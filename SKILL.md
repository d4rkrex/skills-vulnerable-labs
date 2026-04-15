---
name: skills-vulnerable-labs
description: Intentionally vulnerable Claude/Cline Skills for security training and testing
metadata:
  author: Manuel Roldan (mroldan@veritran.com)
  version: 1.0.0
  license: MIT
  repository: https://github.com/d4rkrex/skills-vulnerable-labs
tags:
  - security
  - training
  - vulnerable
  - testing
  - educational
dependencies:
  - bash
  - python3
  - node
  - git
---

# Skills Vulnerable Labs 🛡️

A collection of **intentionally vulnerable** Cline/Claude Skills for security training, testing security scanners like V-Trust, and learning about common vulnerabilities in AI agent skills.

> ⚠️ **WARNING**: These skills contain intentional security vulnerabilities. **DO NOT** use them in production environments or with real data.

## 🎯 Purpose

This repository provides:
- **10 Vulnerable Skills** covering critical CWEs
- **Secure Versions** for comparison
- **Educational Content** for learning
- **Security Tool Testing** capabilities

## 📚 Vulnerability Catalog

| # | Vulnerability | CWE | Severity | Location |
|---|---------------|-----|----------|----------|
| 01 | Command Injection | CWE-77 | CRITICAL | `skills/01-command-injection/` |
| 02 | Path Traversal | CWE-22 | CRITICAL | `skills/02-path-traversal/` |
| 03 | Hardcoded Credentials | CWE-798 | CRITICAL | `skills/03-hardcoded-credentials/` |
| 04 | Arbitrary Code Execution | CWE-94 | CRITICAL | `skills/04-arbitrary-code-execution/` |
| 05 | SSRF | CWE-918 | HIGH | `skills/05-ssrf/` |
| 06 | Unsafe Deserialization | CWE-502 | HIGH | `skills/06-unsafe-deserialization/` |
| 07 | XXE | CWE-611 | HIGH | `skills/07-xxe/` |
| 08 | Dependency Confusion | CWE-829 | MEDIUM | `skills/08-dependency-confusion/` |
| 09 | Excessive Permissions | CWE-250 | MEDIUM | `skills/09-excessive-permissions/` |
| 10 | Information Disclosure | CWE-532 | MEDIUM | `skills/10-information-disclosure/` |

## 🚨 Security Warnings

- This repository is for **educational purposes ONLY**
- Contains **INTENTIONALLY VULNERABLE** code
- DO NOT deploy in production
- DO NOT use with real data
- Only use in isolated testing environments

## 📖 Usage

### Testing with V-Trust

```bash
# Scan all vulnerable skills
vtrust scan https://github.com/d4rkrex/skills-vulnerable-labs

# Expected detection: 10 vulnerabilities
# - 4 CRITICAL (Command Injection, Path Traversal, Hardcoded Credentials, Arbitrary Code Exec)
# - 3 HIGH (SSRF, Unsafe Deserialization, XXE)
# - 3 MEDIUM (Dependency Confusion, Excessive Permissions, Info Disclosure)
```

### Manual Testing

```bash
# Clone repository
git clone https://github.com/d4rkrex/skills-vulnerable-labs.git
cd skills-vulnerable-labs

# Each skill has documentation
cat skills/01-command-injection/README.md

# Compare vulnerable vs secure
diff -u skills/01-command-injection/vulnerable/SKILL.md \
        skills/01-command-injection/secure/SKILL.md
```

## 📜 License

MIT License - See [LICENSE](LICENSE)

## ⚖️ Legal Notice

By using this repository, you agree to use it **responsibly and ethically** for security research and education purposes only.

---

**Repository Maintainer**: Manuel Roldan (mroldan@veritran.com)  
**Created**: 2026-04-13  
**Last Updated**: 2026-04-15
