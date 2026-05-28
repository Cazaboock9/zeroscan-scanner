# ZeroScan

**Supply Chain Security Scanner**

```
╔══════════════════════════════════════════════════════════╗
║  ZeroScan v0.1.0  —  Supply Chain Security Scanner       ║
╚══════════════════════════════════════════════════════════╝
```

---

## Overview

**ZeroScan** is a free, open-source supply chain security scanner.

### What it does:

- Malicious Package Detection — Checks npm/PyPI/Composer packages against blocklists
- Credential Exposure Detection — Finds API keys, tokens, passwords in code
- Threat Pattern Matching — Detects ATT&CK TTP patterns
- Obfuscation Detection — Finds encoded/obfuscated payloads

---

## Installation

### From source

```bash
git clone https://github.com/Cazaboock9/zeroscan-scanner
cd zeroscan-scanner/zeroscan-py
python3 main.py --help
```

### Make it executable

```bash
chmod +x main.py
./main.py --help
```

---

## Usage

```bash
# Scan a file
python3 main.py scan package.json

# Scan a directory
python3 main.py scan-dir ./my-project

# Check if a specific package is malicious
python3 main.py check @openclaw-ai/openclawai

# Show version
python3 main.py version
```

---

## Output Format

```json
{
  "scanner": "zeroscan",
  "version": "0.1.0",
  "timestamp": "2026-05-28T17:30:00Z",
  "scan_type": "file",
  "target": "package.json",
  "results": {
    "threats": [
      {
        "type": "malicious_package",
        "severity": "critical",
        "confidence": 0.95,
        "file": "package-lock.json",
        "description": "@openclaw-ai/openclawai is a known RAT",
        "recommended_action": "Remove immediately"
      }
    ],
    "summary": {
      "total_items_scanned": 234,
      "threats_found": 1,
      "critical": 1,
      "high": 0
    }
  }
}
```

---

## Known Malicious Packages Detected

- @openclaw-ai/openclawai
- durabletask
- node-ipc
- pytorch-lightning
- axios
- dydx-packages

---

## Security Features

- DoS Protection — 50MB max file, rate limiting
- Input Validation — Path traversal prevention
- Sandbox Execution — No code execution of scanned files
- JSON Output — Structured for easy integration

---

## Contributing

Contributions are welcome! ZeroScan is an open project and we appreciate any help from the security community.

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

---

## Disclaimer

> **EXPERIMENTAL SOFTWARE**
>
> ZeroScan is experimental. Run in isolated, disposable environments. Security vulnerabilities should be expected. Always verify critical findings manually.

---

## License

Apache License 2.0 — See [LICENSE](../LICENSE)

---

**ZeroScan** — Protecting AI agents from supply chain threats

Follow the author: https://x.com/dagomint
