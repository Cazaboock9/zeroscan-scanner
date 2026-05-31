# ZeroScan

**Supply Chain Security Scanner — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║ ZeroScan v0.5.0 PoC — Supply Chain Security Scanner ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a proof-of-concept supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

**Key Features:**
- Version-aware detection (checks package + version when provided)
- Exact-match blocklist via `std.mem.eql()` — no typosquat or version-range detection yet (see Limitations)
- Clean separation of logic (`classify()`) and I/O (`main()`)
- 10 known-malicious packages + 1 version-specific case (axios), all with references
- 15 tests that verify the scanner logic itself (including negative cases), not just stdlib
- Compiles to a native binary (~4.4KB)
- Open source under Apache License 2.0

## Installation

```bash
# Install ZeroLang v0.2.0+ compiler
curl -fsSL https://zerolang.ai/install.sh | bash

# Clone and build
git clone https://github.com/Cazaboock9/zeroscan-scanner
cd zeroscan-scanner
zero build . --emit exe --out zeroscan
chmod +x zeroscan
```

## Usage

```bash
# Show banner and blocklist
./zeroscan

# Check a package (name-only)
./zeroscan axios
# Output: WARNING (axios itself is safe — only specific versions were compromised)

# Check a package with version
./zeroscan axios 1.14.1
# Output: MALICIOUS: axios@1.14.1

# Check a safe version
./zeroscan axios 1.7.9
# Output: CLEAN: axios@1.7.9

# Check a clean package
./zeroscan react
# Output: CLEAN: react
```

## Blocklist (10 packages + 1 version-specific)

### CRITICAL

| Package | Reference |
|---------|-----------|
| @openclaw-ai/openclawai | AI agent RAT |
| event-stream | CVE-2018-16492 |
| flatmap-stream | event-stream dependency |

### HIGH — Supply Chain Hijacks (2021)

| Package | Reference |
|---------|-----------|
| ua-parser-js | Hijacked 2021 |
| coa | Hijacked 2021 |
| rc | Hijacked 2021 |

### MEDIUM — Sabotage/Protestware

| Package | Reference |
|---------|-----------|
| colors | Author sabotage 2022 |
| faker | Author sabotage 2022 |
| node-ipc | Protestware with geo-targeting 2022 |

### VERSION-SPECIFIC

| Package | Malicious Versions |
|---------|-------------------|
| axios | 1.14.1, 0.30.4 |

**Note on axios:** Axios itself is NOT malicious — only versions 1.14.1 and 0.30.4 were compromised (via the plain-crypto-js@4.2.1 dependency, March 2026). Running `zeroscan axios` returns a WARNING with guidance to specify a version.

## Version History

### v0.5.0 — Current (2026-05-30)
- Reverted false positives introduced by an earlier blocklist expansion: `polyfill`, `deep-extend`, `systeminformation`, `jwt-decode`, `m3o` are legitimate packages and are no longer flagged by name
- Added negative tests asserting those packages return CLEAN, to prevent regressions
- Blocklist holds 10 known-malicious packages + the axios version-specific case
- 15 tests total

### v0.4.0 (2026-05-29)
- Version-aware detection: `zeroscan <pkg>` vs `zeroscan <pkg> <version>`
- axios by name = WARNING (not MALICIOUS)
- Pure `classify()` function + I/O in `main()`
- 6 tests verifying classify() logic

### v0.3.0 (2026-05-29)
- CLEAN output for unknown packages

### v0.2.0 (2026-05-28)
- `std.mem.eql()` exact string matching

### v0.1.0 — Initial PoC
- Length-based detection (deprecated)

## Architecture

```
classify(pkg, version, has_version) -> i32
├── 0 = CLEAN
├── 1 = WARNING (needs version to determine)
└── 2 = MALICIOUS

main(world)
├── Parse args
├── Call classify()
├── Print result based on verdict
└── I/O stays in main() — World never passed as parameter
```

**Why this architecture?**
- `classify()` is pure logic — easy to test
- `main()` handles all I/O — the World capability never leaks to helper functions
- This pattern works around a ZeroLang v0.2.0 direct-backend limitation (World as a function parameter triggers BLD004)

## Testing

```bash
zero check . # Validate syntax
zero test . # Run 15 tests
```

Tests verify `classify()` logic, in both directions:

**Should be flagged:**
- `axios 1.14.1` → MALICIOUS (compromised version)
- `axios 0.30.4` → MALICIOUS (compromised version)
- `axios` (no version) → WARNING
- `event-stream`, `flatmap-stream`, `ua-parser-js`, `colors`, `node-ipc` → MALICIOUS

**Should NOT be flagged (negative tests):**
- `axios 1.7.9` → CLEAN (safe version)
- `react`, `lodash` → CLEAN (not in blocklist)
- `jwt-decode`, `systeminformation`, `polyfill`, `deep-extend` → CLEAN (legitimate packages)

## Limitations

This is a proof of concept. The detection model is an exact-name (and exact-version) blocklist. That has real consequences:

| Limitation | Impact |
|------------|--------|
| Exact-match only | No typosquatting detection (e.g. `event-strea`), no fuzzy matching, no version *ranges* — only the exact strings listed |
| Blocklist-based | Only known packages are caught; novel/unknown malicious packages return CLEAN |
| No install-script / dependency analysis | Does not inspect package contents, transitive deps, or install hooks |
| World as function parameter | BLD004 on direct backend — I/O stays in main() |
| std.fs / std.http | Hosted target only (linux-x64); TAR002 on non-hosted targets |

"No false positives" here means the exact-match approach will not misflag a package that isn't on the list — it does not mean comprehensive detection. A real scanner would add typosquat detection, version-range matching, and content analysis.

### std.fs / std.http — Target-Dependent, Not "Denied"

The `filesystem` and `network` "denied" in `zero check --json` refers to the **compile-time evaluator**, not the runtime of your compiled binary. The compile-time evaluator cannot access fs/network when evaluating constants — that's intentional for security during compilation.


**Your binary at runtime is different.** On hosted targets (linux-x64), `std.fs` and `std.http` work. The ZeroLang repo itself has examples using them:

```zero
// examples/cli-file.0 — uses std.fs.writeBytes and std.fs.exists
// std-path-io.0 — uses std.fs for file operations
```

The `zero check --json` output confirms linux-x64 has the capability:

```json
{
  "capabilities": ["memory", "stdio", "args", "env", "fs", "net", "proc", "time", "rand"]
}
```


**What this means for ZeroScan:** We chose not to use `std.fs`/`std.http` — not because they're forbidden, but because our current architecture (exact-match blocklist) doesn't need them. A future version could read blocklists from disk or fetch CVE data via HTTP.

## Why ZeroLang?

ZeroLang is designed for AI agents. This project explores:
- Building functional tooling in a new, agent-native language
- Exact string matching via std.mem.eql
- Small native binary size
- Real-world constraints when building practical tools on a pre-1.0 toolchain
- Community feedback to the language team

## License

Apache License 2.0

---

ZeroScan — A ZeroLang proof of concept for supply chain awareness

Follow the author: https://x.com/dagomint
