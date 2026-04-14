# VTrust Skills Vulnerable Labs 🛡️

A collection of intentionally vulnerable Cline/Claude Skills for security training, testing security tools, and learning about common vulnerabilities in AI agent skills.

> ⚠️ **WARNING**: These skills contain intentional security vulnerabilities. **DO NOT** use them in production environments or with real data.

## 🎯 Purpose

Similar to [mcp-breach-to-fix-labs](https://github.com/PawelKozy/mcp-breach-to-fix-labs) for MCP servers, this repository provides:

- **10 Vulnerable Skills** - Each with a common security vulnerability
- **Secure Versions** - Fixed implementations for comparison
- **Educational Content** - Explanation of each vulnerability and mitigation
- **Security Tool Testing** - Test bed for tools like [V-Trust](https://github.com/Veritran/V-Trust)

## 📚 Vulnerability Catalog

| # | Vulnerability | CWE | Risk | Description |
|---|--------------|-----|------|-------------|
| 01 | Command Injection | CWE-77 | 🔴 Critical | Skill executes shell commands without sanitization |
| 02 | Path Traversal | CWE-22 | 🔴 Critical | File operations without path validation |
| 03 | Hardcoded Credentials | CWE-798 | 🔴 Critical | API keys and secrets in code |
| 04 | Arbitrary Code Execution | CWE-94 | 🔴 Critical | Dynamic code execution from user input |
| 05 | Server-Side Request Forgery | CWE-918 | 🟠 High | Unvalidated HTTP requests |
| 06 | Unsafe Deserialization | CWE-502 | 🟠 High | YAML/JSON parsing without validation |
| 07 | XML External Entity (XXE) | CWE-611 | 🟠 High | XML parsing with external entities enabled |
| 08 | Dependency Confusion | CWE-829 | 🟡 Medium | Malicious package substitution |
| 09 | Excessive Permissions | CWE-250 | 🟡 Medium | Requesting unnecessary system access |
| 10 | Information Disclosure | CWE-532 | 🟡 Medium | Logging sensitive data |

## 🗂️ Repository Structure

```
.
├── README.md
├── SECURITY.md
├── skills/
│   ├── 01-command-injection/
│   │   ├── vulnerable/
│   │   │   └── SKILL.md
│   │   ├── secure/
│   │   │   └── SKILL.md
│   │   └── README.md
│   ├── 02-path-traversal/
│   │   ├── vulnerable/
│   │   ├── secure/
│   │   └── README.md
│   └── ... (03-10)
├── tests/
│   └── test_detection.py
└── docs/
    ├── LEARNING_PATHS.md
    └── MITIGATION_GUIDE.md
```

## 🚀 Quick Start

### Testing with V-Trust

```bash
# Scan all vulnerable skills
vtrust scan https://github.com/Veritran/vtrust-skills-vulnerable-labs

# Scan specific vulnerability
vtrust scan https://github.com/Veritran/vtrust-skills-vulnerable-labs --path skills/01-command-injection/vulnerable
```

### Manual Testing

```bash
# Clone repository
git clone https://github.com/Veritran/vtrust-skills-vulnerable-labs.git
cd vtrust-skills-vulnerable-labs

# Each skill has documentation
cat skills/01-command-injection/README.md

# Compare vulnerable vs secure
diff -u skills/01-command-injection/vulnerable/SKILL.md \
        skills/01-command-injection/secure/SKILL.md
```

## 📖 Learning Paths

### 🎓 Beginner Path
1. Information Disclosure (10)
2. Excessive Permissions (09)
3. Hardcoded Credentials (03)

### 🎓 Intermediate Path
4. Path Traversal (02)
5. SSRF (05)
6. Unsafe Deserialization (06)

### 🎓 Advanced Path
7. Command Injection (01)
8. Arbitrary Code Execution (04)
9. XXE (07)
10. Dependency Confusion (08)

## 🛡️ Detection Examples

Each vulnerability includes:
- **OWASP Category** mapping
- **STRIDE** threat classification
- **Detection Patterns** for SAST tools
- **Exploitation Steps**
- **Remediation Guide**

Example from Command Injection:

```yaml
---
name: file-search
description: Search files using grep with user-provided patterns
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - filesystem
  - search
  - VULNERABLE
dependencies:
  - bash
---

# File Search Skill

## Usage

Search files by pattern:
```bash
grep "$USER_PATTERN" /path/to/files/*
```

⚠️ **VULNERABILITY**: Command injection via unquoted variable expansion
```

## 🧪 Validation

Run security tests:

```bash
# Test detection rate
python tests/test_detection.py

# Expected output:
# ✓ Command Injection detected
# ✓ Path Traversal detected
# ✓ Hardcoded Credentials detected
# ...
# Score: 10/10 vulnerabilities detected
```

## 🤝 Contributing

Found a new vulnerability pattern? Want to add more skills? See [CONTRIBUTING.md](CONTRIBUTING.md)

## 📜 License

MIT License - See [LICENSE](LICENSE)

## ⚖️ Legal Notice

This repository contains **intentionally vulnerable code** for educational purposes only.

- **DO NOT** deploy these skills in production
- **DO NOT** use with real user data
- **DO NOT** use for malicious purposes
- Only use in controlled, isolated environments

By using this repository, you agree to use it responsibly and ethically.

## 🔗 Related Projects

- [mcp-breach-to-fix-labs](https://github.com/PawelKozy/mcp-breach-to-fix-labs) - Vulnerable MCP servers
- [V-Trust](https://github.com/Veritran/V-Trust) - Security scanner for MCP/Skills
- [OWASP Vulnerable Web Apps](https://owasp.org/www-project-vulnerable-web-applications-directory/)

## 📧 Contact

Questions? Security concerns? Contact: appsec@veritran.com

---

**Remember**: These are intentionally vulnerable skills for learning purposes. Always prioritize security in production code.
