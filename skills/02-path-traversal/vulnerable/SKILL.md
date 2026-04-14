---
name: config-reader
description: Read configuration files from project directory
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - filesystem
  - config
  - VULNERABLE
dependencies:
  - bash
  - cat
---

# Config Reader Skill (VULNERABLE VERSION)

⚠️ **WARNING**: This skill contains a CRITICAL path traversal vulnerability (CWE-22)

## Description

Reads configuration files from the project directory. Allows users to specify config file paths.

## Usage

```bash
# Read application config
cat /project/config/app.yaml

# Read database config
cat /project/config/database.json
```

## Implementation

```bash
#!/bin/bash

FILENAME="$1"
BASE_DIR="/project/config"

# VULNERABLE: No path validation
cat "$BASE_DIR/$FILENAME"
```

## 🔴 VULNERABILITY: Path Traversal (CWE-22)

### Exploitation

```bash
# Read /etc/passwd
Input: ../../etc/passwd
Result: cat /project/config/../../etc/passwd

# Read SSH keys
Input: ../../../root/.ssh/id_rsa

# Read application secrets
Input: ../../app/secrets.env
```

### Impact

- Read arbitrary files
- Access credentials
- Expose source code
- Information disclosure

**DREAD Score**: 8.5/10 (Critical)

## 🔒 Secure Version

See `../secure/SKILL.md`
