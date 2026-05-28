# ZeroScan

**Supply Chain Security Scanner — Built with ZeroLang**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.1.0  —  Supply Chain Security Scanner   ║
╚══════════════════════════════════════════════════════════╝
```

## Overview

ZeroScan is a free, open-source supply chain security scanner written in **ZeroLang** — a programming language designed for AI agents.

### Features

- Detects malicious packages in npm/PyPI/Composer dependencies
- ZeroLang syntax optimized for AI agent consumption
- JSON-compatible output structure

## Installation

### Build from source

```bash
# Install ZeroLang compiler
curl -fsSL https://zerolang.ai/install.sh | bash

# Clone and build
git clone https://github.com/Cazaboock9/zeroscan-scanner
cd zeroscan-scanner
zero build --target host --emit exe --out zeroscan src/main.0
./zeroscan
```

## Usage

```bash
./zeroscan
```

Output:
```
ZeroScan v0.1.0
Supply Chain Security Scanner
Built with ZeroLang

Usage: zeroscan check <package>

Blocklist:
  @openclaw-ai/openclawai - CRITICAL
  durabletask - CRITICAL
  node-ipc - HIGH
  pytorch-lightning - CRITICAL
  axios - HIGH
  dydx-packages - HIGH
```

## Known Malicious Packages

| Package | Severity |
|---------|----------|
| @openclaw-ai/openclawai | CRITICAL |
| durabletask | CRITICAL |
| node-ipc | HIGH |
| pytorch-lightning | CRITICAL |
| axios | HIGH |
| dydx-packages | HIGH |

## Contributing

Contributions welcome! See GitHub for details.

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint
