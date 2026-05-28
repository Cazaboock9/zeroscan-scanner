# ZeroScan

**Supply Chain Security Scanner — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.1.0  —  Supply Chain Security Scanner   ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a free, open-source supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

## Features

- Detects **26 malicious packages** by string length
- 100% ZeroLang — compiles to native binary
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
./zeroscan @openclaw-ai/openclawai
./zeroscan event-stream
./zeroscan durabletask
./zeroscan node-ipc
./zeroscan pytorch-lightning
./zeroscan axios
```

## Blocklist (26 packages)

| Package | Severity | Length |
|--------|----------|--------|
| @openclaw-ai/openclawai | CRITICAL | 23 |
| durabletask | CRITICAL | 11 |
| event-stream | CRITICAL | 12 |
| flatmap-stream | CRITICAL | 14 |
| pytorch-lightning | CRITICAL | 17 |
| node-ipc | HIGH | 8 |
| axios | HIGH | 5 |
| dydx-packages | HIGH | 13 |
| systeminformation | MEDIUM | 25 |
| crypto-js | MEDIUM | 9 |
| is-promise | MEDIUM | 10 |
| + 15 more typosquatting packages | LOW | various |

## Technical Notes

ZeroLang v0.1.4 does not support string comparison operators (`==` with String type). This implementation uses string length as a unique identifier for package names.

**Known False Positive:** Packages with same length as blocklist items (e.g., `react` = 5 same as `axios`) will trigger alerts. This is a limitation of the length-based workaround.

## Testing

```bash
zero test tests/blocklist_test.0
```

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint
