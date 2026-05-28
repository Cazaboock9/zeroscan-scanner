# ZeroScan

**Supply Chain Security Scanner PoC — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.2.0 PoC  —  Supply Chain Security Scanner     ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a proof-of-concept supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

**Key Innovation:** Uses `std.mem.eql()` for exact string matching — **zero false positives**.

## Features

- Exact string matching with `std.mem.eql()` — no false positives
- 10 verified malicious packages with CVE references
- Compiles to native binary (~3.5KB)
- Open source under Apache License 2.0

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

## Testing

```bash
zero test tests/blocklist_test.0
```

## ⚠️ LIMITATIONS

**v0.1.4 Feedback — Help Wanted!**

This PoC was built pushing ZeroLang v0.1.4 to its limits. Here's what we hit:

| Limitation | Impact | Workaround |
|------------|--------|------------|
| No `mut` variables | Can't track if a package was already matched | Multiple `if` checks per package |
| No `String` as function parameter | Can't extract helper functions | All logic in `main` |
| No early `ret` in nested context | Clean packages produce no output | Silent for unknown packages |

**The "Clean Package Silent" Issue:**

When checking an unknown package (not in blocklist), ZeroScan produces **no output**. This is because without mutable variables or early returns in helper functions, we can't implement a `found` flag pattern.

**Workaround:** If no match is found, the scanner stays silent. For a production tool, this would need `let mut found = false` or `ret` statements in helper functions.

**These limitations are documented bugs for the ZeroLang team — not failures of the implementation.**

## Why ZeroLang?

ZeroLang is designed for AI agents. This project demonstrates:
- Functional security tooling in a new language
- Exact string matching via std.mem.eql
- Small binary size (~3.5KB)
- Real-world constraints when building practical tools

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint