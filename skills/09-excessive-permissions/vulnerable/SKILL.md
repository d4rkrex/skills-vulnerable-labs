---
name: system-manager
description: Manage system resources and services
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - system
  - permissions
  - VULNERABLE
dependencies:
  - sudo
  - docker
  - systemctl
---

# System Manager Skill (VULNERABLE VERSION)

⚠️ **WARNING**: Excessive permissions (CWE-250)

## Requested Permissions

```yaml
permissions:
  filesystem:
    - read: "/**"           # EXCESSIVE: Full filesystem read
    - write: "/**"          # EXCESSIVE: Full filesystem write
  network:
    - "*:*"                 # EXCESSIVE: All ports, all protocols
  execution:
    - sudo                  # EXCESSIVE: Root access
    - docker                # EXCESSIVE: Container escape possible
  environment:
    - ALL_ENV_VARS         # EXCESSIVE: Access to all secrets
```

## Implementation

```bash
#!/bin/bash

# VULNERABLE: Runs with excessive privileges
sudo systemctl restart nginx    # Only needs nginx control
sudo docker exec -it app bash   # Full container access
sudo chmod 777 /var/log         # Overly permissive
```

## 🔴 VULNERABILITY

### Issues

1. **Principle of Least Privilege Violation**
2. **Unnecessary Root Access**
3. **Broad Filesystem Access**
4. **Container Escape Risk**
5. **Privilege Escalation Potential**

### Impact

- Can compromise entire system
- Access all user data
- Modify critical files
- Escape containers
- Lateral movement

**DREAD Score**: 7.0/10 (Medium)
