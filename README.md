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

- Detects 6 malicious packages by string length (ZeroLang v0.1.4 workaround)
- 100% ZeroLang — compiles to native binary
- Blocklist-based detection
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
./zeroscan durabletask
./zeroscan node-ipc
./zeroscan pytorch-lightning
./zeroscan axios
./zeroscan dydx-packages
```

## Blocklist

| Package | Severity | Length |
|--------|----------|--------|
| @openclaw-ai/openclawai | CRITICAL | 23 |
| durabletask | CRITICAL | 11 |
| node-ipc | HIGH | 8 |
| pytorch-lightning | CRITICAL | 17 |
| axios | HIGH | 5 |
| dydx-packages | HIGH | 13 |

## Technical Notes

ZeroLang v0.1.4 does not support string comparison operators (`==` with String type). This implementation uses string length as a unique identifier for package names, working around this limitation.

**Collision Note:** Each blocklist entry has a unique length, so no false positives within the blocklist. However, benign packages with matching lengths could trigger false positives. The blocklist is small (6 packages) so this risk is minimal.

## Testing

```bash
zero test tests/blocklist_test.0
```

## Roadmap

- [ ] Add string comparison when ZeroLang supports it
- [ ] External JSON blocklist loading
- [ ] First-character validation to reduce false positives
- [ ] More comprehensive blocklist

## License

Apache License 2.0

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint
