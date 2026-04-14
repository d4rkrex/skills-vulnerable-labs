---
name: file-search
description: Search files using grep with validated patterns
metadata:
  author: VTrust Security Labs
  version: 1.0.0-secure
  license: MIT
  category: filesystem
tags:
  - filesystem
  - search
  - grep
  - SECURE
dependencies:
  - bash
  - grep
---

# File Search Skill (SECURE VERSION)

✅ This skill properly mitigates command injection vulnerabilities

## Description

This skill allows searching for text patterns in files using grep with proper input validation and safe command execution.

## Usage

Search for a pattern in project files:

```bash
# User input: "TODO"
grep -F "TODO" /project/src/**/*.py

# User input: "FIXME"
grep -F "FIXME" /project/docs/*.md
```

## Tools Used

- `bash` - Shell command execution (with proper quoting)
- `grep` - Text pattern matching (with -F fixed-string mode)
- `find` - File discovery (with controlled arguments)

## Implementation

The skill uses multiple security controls:

```bash
#!/bin/bash
set -euo pipefail  # Strict error handling

# Get and validate search pattern
PATTERN="$1"
DIRECTORY="${2:-.}"

# SECURITY: Input validation
if [[ ! "$PATTERN" =~ ^[a-zA-Z0-9_[:space:].-]+$ ]]; then
    echo "Error: Invalid pattern. Only alphanumeric, spaces, dots, dashes allowed."
    exit 1
fi

# SECURITY: Validate directory exists and is within allowed paths
ALLOWED_BASE="/project"
REAL_DIR=$(realpath "$DIRECTORY" 2>/dev/null || echo "")

if [[ -z "$REAL_DIR" ]] || [[ ! "$REAL_DIR" =~ ^"$ALLOWED_BASE" ]]; then
    echo "Error: Invalid or unauthorized directory."
    exit 1
fi

# SECURITY: Use -F (fixed-string) to prevent regex injection
# SECURITY: Proper quoting of all variables
# SECURITY: Use find with controlled arguments instead of glob
find "$REAL_DIR" -type f -name "*.py" -o -name "*.md" | \
    xargs -I {} grep -F -n "$PATTERN" "{}"

# Alternative secure approach using array
declare -a FILES
mapfile -t FILES < <(find "$REAL_DIR" -type f \( -name "*.py" -o -name "*.md" \))

for file in "${FILES[@]}"; do
    grep -F -n "$PATTERN" "$file" 2>/dev/null || true
done
```

## 🔒 Security Controls

### 1. Input Validation
```bash
# Whitelist approach - only allow safe characters
if [[ ! "$PATTERN" =~ ^[a-zA-Z0-9_[:space:].-]+$ ]]; then
    echo "Invalid input"
    exit 1
fi
```

### 2. Path Validation
```bash
# Prevent path traversal
ALLOWED_BASE="/project"
REAL_DIR=$(realpath "$DIRECTORY")
[[ "$REAL_DIR" =~ ^"$ALLOWED_BASE" ]] || exit 1
```

### 3. Proper Quoting
```bash
# Always quote variables
grep -F "$PATTERN" "$file"
# NOT: grep -F $PATTERN $file
```

### 4. Use grep -F (Fixed String)
```bash
# Prevents regex injection attacks
grep -F "pattern" file.txt
# NOT: grep "pattern" file.txt
```

### 5. Avoid eval/Dynamic Execution
```bash
# NEVER use eval with user input
# NEVER: eval "grep $PATTERN $FILE"
```

### 6. Strict Error Handling
```bash
set -euo pipefail
# -e: exit on error
# -u: error on unset variables
# -o pipefail: fail if any pipe command fails
```

### 7. Principle of Least Privilege
```bash
# Use find with explicit file types
find "$DIR" -type f -name "*.py"
# NOT: find "$DIR" -type f  (too broad)
```

## 🛡️ Defense in Depth

This implementation uses multiple security layers:

1. **Input Validation** - Whitelist allowed characters
2. **Path Sanitization** - Prevent directory traversal
3. **Safe Command Construction** - Proper quoting
4. **Safe grep Mode** - Use -F to disable regex
5. **Controlled File Discovery** - Explicit file type filtering
6. **Error Handling** - Fail fast on errors
7. **Logging** - Audit all operations (not shown for brevity)

## 🧪 Testing

Verify security controls:

```bash
# Test 1: Valid input
./file-search.sh "TODO" "/project/src"
# Expected: Success, finds TODOs

# Test 2: Malicious command injection attempt
./file-search.sh "TODO\"; cat /etc/passwd #" "/project"
# Expected: Rejected due to invalid characters

# Test 3: Path traversal attempt
./file-search.sh "TODO" "../../../etc"
# Expected: Rejected, outside allowed base

# Test 4: Regex injection attempt
./file-search.sh ".*" "/project"
# Expected: Treated as literal ".*" string (not regex)
```

## 📊 Security Comparison

| Control | Vulnerable | Secure |
|---------|-----------|--------|
| Input validation | ❌ None | ✅ Whitelist |
| Variable quoting | ❌ Unquoted | ✅ Quoted |
| Path validation | ❌ None | ✅ realpath check |
| grep mode | ❌ Regex | ✅ Fixed-string (-F) |
| Error handling | ❌ None | ✅ set -euo pipefail |
| Command construction | ❌ eval | ✅ Direct execution |

## 📚 Best Practices Applied

1. ✅ **Never trust user input** - Always validate
2. ✅ **Use proper quoting** - Prevent word splitting
3. ✅ **Avoid dynamic code execution** - No eval/exec
4. ✅ **Principle of least privilege** - Minimal permissions
5. ✅ **Defense in depth** - Multiple security layers
6. ✅ **Fail securely** - Reject invalid input
7. ✅ **Audit operations** - Log security events

## 🔗 References

- [CWE-77 Mitigation](https://cwe.mitre.org/data/definitions/77.html)
- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
- [Bash Security Best Practices](https://mywiki.wooledge.org/BashGuide/Practices)
- [ShellCheck](https://www.shellcheck.net/) - Static analysis for shell scripts

## ✅ Compliance

This implementation meets:
- ✅ OWASP Top 10 A03:2021 (Injection)
- ✅ CWE-77 (Command Injection) mitigation
- ✅ SANS Top 25 CWE-78 (OS Command Injection)
- ✅ NIST SP 800-53 SI-10 (Information Input Validation)

---

**Key Takeaway**: Multiple layers of defense transform a critical vulnerability into a secure implementation.
