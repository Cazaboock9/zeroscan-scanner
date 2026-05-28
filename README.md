# ZeroScan

**Supply Chain Security Scanner — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.2.0 PoC  —  Supply Chain Security Scanner   ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a proof-of-concept supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

**Key Innovation:** Uses `std.mem.eql()` for exact string matching — **zero false positives**.

## Features

- Exact string matching with `std.mem.eql()` — no false positives
- 10 verified malicious packages with CVE references
- Compiles to native binary (~3.5KB)
- MIT Licensed

## Installation

```bash
# Install ZeroLang compiler
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
./zeroscan node-ipc
./zeroscan colors
```

## Blocklist (10 verified packages)

| Package | Severity | Reference |
|---------|----------|----------|
| @openclaw-ai/openclawai | CRITICAL | AI agent RAT |
| event-stream | CRITICAL | CVE-2018-16492 |
| flatmap-stream | CRITICAL | event-stream dep |
| node-ipc | HIGH | Protestware 2022 |
| ua-parser-js | HIGH | Hijacked 2021 |
| coa | HIGH | Hijacked 2021 |
| rc | HIGH | Hijacked 2021 |
| colors | MEDIUM | Sabotage 2022 |
| faker | MEDIUM | Sabotage 2022 |

## Technical Notes

Built with ZeroLang v0.1.4 using `std.mem.eql()` for exact string comparison — no length-based detection, no false positives from name length collisions.

## Testing

```bash
zero test tests/blocklist_test.0
```

## Why ZeroLang?

ZeroLang is designed for AI agents. This project demonstrates:
- Functional security tooling in a new language
- Exact string matching via std.mem.eql
- Small binary size (~3.5KB)

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint
