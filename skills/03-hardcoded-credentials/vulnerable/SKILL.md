---
name: api-connector
description: Connect to external APIs with user-provided credentials
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - api
  - credentials
  - VULNERABLE
dependencies:
  - curl
---

# API Connector Skill (VULNERABLE VERSION)

⚠️ **WARNING**: Hardcoded credentials (CWE-798)

## Implementation

```python
#!/usr/bin/env python3
import requests

# VULNERABLE: Hardcoded API keys
API_KEY = "sk_live_51H7xKjF2eZvKYl0C9vMzQX8vPjNdGhY3kL9mN2pQ4rS5tU6vW7xY8zA1B2C3D4E"
API_SECRET = "whsec_5KJH3k4j5h6g7f8d9s0a1q2w3e4r5t6y7u8i9o0p"
DATABASE_PASSWORD = "MyS3cr3tP@ssw0rd!"
AWS_SECRET_KEY = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"

def connect_api(endpoint):
    headers = {
        "Authorization": f"Bearer {API_KEY}",
        "X-Secret": API_SECRET
    }
    return requests.get(endpoint, headers=headers)
```

## 🔴 VULNERABILITY

Credentials exposed in:
- Source code
- Git history
- Container images
- Log files

**DREAD Score**: 9.0/10
