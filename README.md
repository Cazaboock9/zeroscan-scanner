# ZeroScan

**Supply Chain Security Scanner PoC — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.3.0 PoC  —  Supply Chain Security Scanner   ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a proof-of-concept supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

**Key Innovation:** Uses `std.mem.eql()` for exact string matching — **zero false positives**. Now with `var` mutable bindings and proper clean package detection in v0.2.0+.

## Features

- Exact string matching with `std.mem.eql()` — no false positives
- Proper clean package detection (shows "CLEAN: package" for unknown packages)
- 11 verified malicious packages with CVE references
- Compiles to native binary (~4.3KB)
- Open source under Apache License 2.0

## Installation

```bash
# Install ZeroLang v0.2.0+ compiler
curl -fsSL https://zerolang.ai/install.sh | bash

# Clone and build
git clone https://github.com/Cazaboock9/zeroscan-scanner
cd zeroscan-scanner
zero build . --target host --emit exe --out zeroscan
chmod +x zeroscan
```

## Usage

```bash
# Show banner and blocklist
./zeroscan

# Check a package
./zeroscan event-stream
./zeroscan axios
./zeroscan react
```

## Blocklist (11 verified packages)

| Package | Severity | Reference |
|---------|----------|-----------|
| @openclaw-ai/openclawai | CRITICAL | AI agent RAT |
| event-stream | CRITICAL | CVE-2018-16492 |
| flatmap-stream | CRITICAL | event-stream dep |
| axios | HIGH | Cross-platform RAT |
| node-ipc | HIGH | Protestware 2022 |
| ua-parser-js | HIGH | Hijacked 2021 |
| coa | HIGH | Hijacked 2021 |
| rc | HIGH | Hijacked 2021 |
| colors | MEDIUM | Sabotage 2022 |
| faker | MEDIUM | Sabotage 2022 |

## Testing

```bash
zero test tests/blocklist_test.0
```

## v0.3.0 — Updates

- **Proper clean detection**: Unknown packages now show "CLEAN: package" instead of silent output
- **Added axios blocklist entry**: Cross-platform RAT detection
- **Uses v0.2.0 features**: `var found: Bool = false` mutable binding pattern

## Why ZeroLang?

ZeroLang is designed for AI agents. This project demonstrates:
- Functional security tooling in a new language
- Exact string matching via std.mem.eql
- Small binary size (~4.3KB)
- Real-world constraints when building practical tools

## History

- **v0.1.0**: Initial proof-of-concept with length-based detection
- **v0.2.0**: Rewrote with `std.mem.eql()` for exact matching — fixed false positives
- **v0.2.0+**: Discovered v0.2.0 features (`var`, `else`, `while`) enable proper clean package detection
- **v0.3.0**: Full implementation with clean package output, axios added to blocklist

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint