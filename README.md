# ZeroScan

**Supply Chain Security Scanner — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.4.0 PoC  —  Supply Chain Security Scanner ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a proof-of-concept supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

**Key Features:**
- Version-aware detection (checks package + version when provided)
- Exact string matching with `std.mem.eql()` — zero false positives
- Clean separation of logic (`classify()`) and I/O (`main()`)
- 10 verified malicious packages with CVE references
- 6 passing tests that verify the scanner logic (not just stdlib)
- Compiles to native binary (~4.1KB)
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

## Blocklist (10 verified packages)

| Package | Severity | Reference |
|---------|----------|-----------|
| @openclaw-ai/openclawai | CRITICAL | AI agent RAT |
| event-stream | CRITICAL | CVE-2018-16492 |
| flatmap-stream | CRITICAL | event-stream dep |
| node-ipc | HIGH | Protestware 2022 |
| ua-parser-js | HIGH | Hijacked 2021 |
| coa | HIGH | Hijacked 2021 |
| rc | HIGH | Hijacked 2021 |
| colors | MEDIUM | Sabotage 2022 |
| faker | MEDIUM | Sabotage 2022 |
| axios | WARNING | Version-specific (1.14.1, 0.30.4) |

**Note on axios:** Axios itself is NOT malicious — only versions 1.14.1 and 0.30.4 were compromised (plain-crypto-js@4.2.1 dependency). Running `zeroscan axios` gives a WARNING with guidance to specify a version.

## Version History

### v0.4.0 — Current (2026-05-29)
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
zero test .     # Run 6 tests
```

**Tests verify:**
- `axios 1.14.1` → MALICIOUS (compromised version)
- `axios 0.30.4` → MALICIOUS (compromised version)
- `axios 1.7.9` → CLEAN (safe version)
- `axios` (no version) → WARNING (needs version to confirm)
- `event-stream` → MALICIOUS (always malicious)
- `react` → CLEAN (not in blocklist)

## Limitations

| Limitation | Impact | Workaround |
|------------|--------|------------|
| World as function parameter | BLD004 on direct backend | I/O stays in main() |
| std.fs/std.http | Not available in direct backend | See future roadmap |

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