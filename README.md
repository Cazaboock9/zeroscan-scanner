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
- Exact string matching with `std.mem.eql()`
- Clean separation of logic (`classify()`) and I/O (`main()`)
- 11 verified malicious packages (10 by name + axios version-specific)
- 15 passing tests that verify the scanner logic (not just stdlib)
- Compiles to native binary (~4.4KB)
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

## Blocklist (11 verified packages)

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
| node-ipc | Protestware with geo-targeting |

### VERSION-SPECIFIC

| Package | Malicious Versions |
|---------|-------------------|
| axios | 1.14.1, 0.30.4 |

**Note on axios:** Axios itself is NOT malicious — only versions 1.14.1 and 0.30.4 were compromised (plain-crypto-js@4.2.1 dependency). Running `zeroscan axios` gives a WARNING with guidance to specify a version.

## Version History

### v0.5.0 — Current (2026-05-31)
- **11 verified packages** (down from 18 — removed false positives)
- **15 passing tests** (including negative tests for legitimate packages)
- **False positives removed**: jwt-decode, systeminformation, polyfill, deep-extend, m3o
- **Binary**: 4.4KB

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

## Architecture

```
classify(package, version, has_version) → i32
├── return 2  // MALICIOUS
├── return 1  // WARNING (needs version)
└── return 0  // CLEAN

main() → I/O only
├── Parse args
├── Call classify()
└── Output result
```

**Key insight:** `classify()` is a pure function — no World parameter, no I/O. This avoids BLD004 entirely.

## Why ZeroLang?

| Advantage | Explanation |
|-----------|-------------|
| Tiny binary | ~4KB compiled |
| Agent-native | Designed for AI agent workflows |
| Pure functions | No side effects, easy to test |
| Graph introspection | `zero graph dump` for deep analysis |

## License

Apache License 2.0
