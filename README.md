# 🎯 Keen-Dork · Severity Edition

> Advanced Google Dorking Toolkit for Bug Hunters
> Organized by CVSS severity • One-click queries • Recon on steroids

[![License: MIT](https://img.shields.io/badge/License-MIT-orange.svg)](LICENSE)
[![Bughunter Ready](https://img.shields.io/badge/🐞-Bughunter%20Ready-brightgreen)](#)
[![Severity-Based](https://img.shields.io/badge/🔥-Severity%20Sorted-red)](#)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-None-8A2BE2)](#)

## 🚀 Quick Start

Download the repository and open index.html directly in any modern browser.


1. Enter your target domain in the input field
2. Click Apply & Refresh Dorks
3. Click any dork card to execute the query
4. Analyze results responsibly and document findings

## ✨ Features

• 35+ pre-built dorks mapped to 5 severity tiers
• Multi-platform execution: Google, Shodan, crt.sh, Wayback Machine, GitHub
• Live target substitution with automatic domain sanitization
• Glassmorphism UI with animated galaxy canvas background
• Zero dependencies, fully responsive, works completely offline
• One-click execution with secure external linking

## 📊 Severity Tiers

| Badge | Focus | Common Targets |
|-------|-------|----------------|
| 🔴 Critical | Secrets, Keys, PII | env files, AWS credentials, private keys, database strings |
| 🟠 High | Admin, Backups, DevOps | Jenkins, git exposure, SQL dumps, K8s/Terraform configs |
| 🟡 Medium | Errors, Debug, APIs | Stack traces, Swagger/OpenAPI, open redirect parameters |
| 🔵 Low | Portals, Docs, Listings | Login pages, version disclosures, directory indexes |
| ⚪ Info | Recon, Footprinting | crt.sh certificates, Wayback archives, Shodan hosts, GitHub code |

## ⚠️ Responsible Use Policy

+ Authorized targets only with written permission
+ Bug bounty programs operating within defined scope
+ Personal labs, CTF environments, and educational research
- No unauthorized scanning or enumeration
- No credential abuse or data exfiltration
- No destructive testing, brute-force, or DoS attempts

Always comply with target Terms of Service, program guidelines, and local cybersecurity laws.

## 🛠️ Architecture & Stack

Frontend: Vanilla JavaScript, CSS3 Grid/Flexbox, Canvas API
Design: Glassmorphism panels, animated starfield, responsive breakpoints
Performance: Framework-free, lightweight DOM rendering, offline-ready
Compatibility: Chrome, Firefox, Safari, Edge, and modern mobile browsers

## 🤝 Contributing

Found a high-impact dork or want to improve the tool?

1. Fork the repository
2. Create a feature branch
3. Add your dork to the DORKS object inside index.html
4. Include context: target type, expected output, false-positive mitigation
5. Submit a pull request with a clear description and usage notes

## 🔗 Integrated Quick Access

The interface includes built-in one-click shortcuts for:
• Google site-scoped searches
• GitHub code and secret scanning
• Shodan hostname and organization queries
• crt.sh certificate transparency logs
• Wayback Machine historical URL archives
• Security Headers analysis
• OpenBugBounty report lookup
• PublicWWW source code indexing

## 👤 Author

keen-i — Bughunter & Security Researcher
GitHub: https://github.com/keen-i

Crafted for the hunt. Stay sharp. 🕵️‍♂️

## 📜 License & Disclaimer

MIT License. This tool is provided for authorized security research, bug bounty hunting, and educational purposes only. The author assumes no liability for misuse, unauthorized access, or violations of target policies. Use responsibly.
