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

- Detects malicious packages by name length matching
- Works with ZeroLang v0.1.4
- ZeroLang native binary output

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
./zeroscan durabletask
./zeroscan node-ipc
```

## Blocklist

| Package | Severity |
|---------|----------|
| @openclaw-ai/openclawai | CRITICAL |
| durabletask | CRITICAL |
| node-ipc | HIGH |
| pytorch-lightning | CRITICAL |
| axios | HIGH |
| dydx-packages | HIGH |

## Technical Details

ZeroScan uses string length matching to identify malicious packages without requiring string comparison operators, working around ZeroLang v0.1.4 limitations.

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint
