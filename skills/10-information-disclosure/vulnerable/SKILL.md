---
name: user-manager
description: Manage user accounts and authentication
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - users
  - logging
  - VULNERABLE
dependencies:
  - python
---

# User Manager Skill (VULNERABLE VERSION)

⚠️ **WARNING**: Information disclosure via logging (CWE-532)

## Implementation

```python
#!/usr/bin/env python3
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

def login_user(username, password):
    # VULNERABLE: Logging credentials
    logger.info(f"Login attempt: username={username}, password={password}")
    
    # VULNERABLE: Logging tokens
    token = generate_token(username)
    logger.debug(f"Generated token: {token}")
    
    return token

def process_payment(card_number, cvv, amount):
    # VULNERABLE: Logging PII and sensitive financial data
    logger.info(f"Processing payment: card={card_number}, cvv={cvv}, amount={amount}")
    
    # VULNERABLE: Logging full request with secrets
    logger.debug(f"Full request: {locals()}")

def authenticate_api(api_key, secret):
    # VULNERABLE: Logging secrets
    print(f"[DEBUG] API Auth: key={api_key}, secret={secret}")
    
    # VULNERABLE: Exception reveals secrets
    try:
        connect(api_key, secret)
    except Exception as e:
        logger.error(f"Auth failed with key={api_key}, secret={secret}: {e}")
```

## 🔴 VULNERABILITY

### Exposed in Logs

```log
2026-04-13 23:10:15 INFO Login attempt: username=admin, password=MyS3cr3tP@ss!
2026-04-13 23:10:15 DEBUG Generated token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
2026-04-13 23:10:20 INFO Processing payment: card=4532-1234-5678-9010, cvv=123, amount=599.99
2026-04-13 23:10:25 ERROR Auth failed with key=sk_live_abc123, secret=whsec_xyz789
```

### Impact

- Credentials exposed in log files
- PII/PCI data leakage
- Tokens accessible to attackers
- API keys compromised
- Compliance violations (GDPR, PCI-DSS)

**DREAD Score**: 6.5/10 (Medium)
