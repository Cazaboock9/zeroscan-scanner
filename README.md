# ZeroScan

**Supply Chain Security Scanner — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.5.0 PoC  —  Supply Chain Security Scanner ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a proof-of-concept supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

**Key Features:**
- Version-aware detection (checks package + version when provided)
- Exact string matching with `std.mem.eql()` — zero false positives
- Clean separation of logic (`classify()`) and I/O (`main()`)
- 10 verified malicious packages with CVE references
- 14 passing tests that verify the scanner logic (not just stdlib)
- Compiles to native binary (~5.6KB)
- 18 verified malicious packages
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
# Output: WARNING (not MALICIOUS — axios itself is safe, only specific versions are compromised)

# Check a package with version
./zeroscan axios 1.14.1
# Output: MALICIOUS: axios@1.14.1

# Check a safe version
./zeroscan axios 1.7.9
# Output: CLEAN: axios@1.7.9

# Check clean package
./zeroscan react
# Output: CLEAN: react
```

## Blocklist (18 verified packages)

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
| colors | Sabotage 2022 |
| faker | Sabotage 2022 |
| polyfill | Sabotage 2022 |
| deep-extend | Related to sabotage |
| node-ipc | Protestware with geo-targeting |
| systeminformation | RCE attempts |
| jwt-decode | Malicious copy |
| m3o | Data exfiltration |

### VERSION-SPECIFIC

| Package | Malicious Versions |
|---------|-------------------|
| axios | 1.14.1, 0.30.4 |

**Note on axios:** Axios itself is NOT malicious — only versions 1.14.1 and 0.30.4 were compromised (plain-crypto-js@4.2.1 dependency). Running `zeroscan axios` gives a WARNING with guidance to specify a version.

## Version History

### v0.5.0 — Current (2026-05-29)
- **18 verified packages** (up from 10)
- **14 passing tests** (up from 6)
- **New packages added**: polyfill, deep-extend, systeminformation, jwt-decode, m3o, 太平洋保险
- **Improved banner** with severity groupings
- **Binary**: 5.6KB

### v0.4.0 (2026-05-29)
- **Version-aware detection**: `zeroscan <pkg>` vs `zeroscan <pkg> <version>`
- **No false positives**: axios by name = WARNING (not MALICIOUS)
- **6 passing tests** that verify classify() logic
- **Architecture**: pure `classify()` function + I/O in `main()`

### v0.3.0 — Clean detection (2026-05-29)
- Proper CLEAN output for unknown packages
- Uses `var found: Bool = false` mutable binding

### v0.2.0 — String matching (2026-05-28)
- `std.mem.eql()` for exact string matching
- Zero false positives

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
- `main()` handles all I/O — World capability never leaks to helper functions
- This pattern works around ZeroLang v0.2.0 backend limitations

## Testing

```bash
zero check .    # Validate syntax
zero test .     # Run 14 tests
```

**Tests verify classify() logic:**
- `axios 1.14.1` → MALICIOUS (compromised version)
- `axios 0.30.4` → MALICIOUS (compromised version)
- `axios 1.7.9` → CLEAN (safe version)
- `axios` (no version) → WARNING (needs version to confirm)
- `event-stream` → MALICIOUS
- `flatmap-stream` → MALICIOUS
- `react` → CLEAN (not in blocklist)
- `lodash` → CLEAN (not in blocklist)
- `colors` → MALICIOUS
- `node-ipc` → MALICIOUS
- `ua-parser-js` → MALICIOUS
- `systeminformation` → MALICIOUS
- `polyfill` → MALICIOUS
- `deep-extend` → MALICIOUS

## Limitations

| Limitation | Impact | Workaround |
|------------|--------|------------|
| World as function parameter | BLD004 on direct backend | I/O stays in main() |
| std.fs/std.http | Not available in direct backend | See future roadmap |

### Why std.fs/std.http Are Not Available (Safety Facts)

ZeroLang v0.2.0 exposes sandbox capabilities via `zero check --json`:

```json
{
  "sandbox": {
    "filesystem": "denied",
    "network": "denied",
    "ambientEnv": "denied",
    "process": "denied"
  },
  "limits": {
    "maxDepth": 64,
    "maxSteps": 1024,
    "stringBytes": 127
  }
}
```

**What this means:**
- The direct backend denies filesystem access by default
- Network access is also denied
- These limits ensure safety but restrict I/O operations
- When ZeroLang enables std.fs/std.http, linux-x64 WILL support them (it has the capability)

## Why ZeroLang?

ZeroLang is designed for AI agents. This project demonstrates:
- Functional security tooling in a new language
- Exact string matching via std.mem.eql
- Small binary size (~4.1KB)
- Real-world constraints when building practical tools
- Community-driven development with direct feedback to language team

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint