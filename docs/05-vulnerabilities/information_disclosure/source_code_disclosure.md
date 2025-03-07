---
sidebar_position: 6
title: Source code disclosure
---

# Source code disclosure

## Description

The source code for the current page was disclosed by the web server.

## Remediation

Ensure that `.git`, `.svn`, `.htaccess` metadata files are not deployed to the web server or application server, or cannot be accessed.


## Configuration

> CheckId: `information_disclosure/source_code_disclosure`

### Options

- *size_threshold* : The threshold size indicating whether a response is small or not.

- *diff_threshold* : The percentage by which 2 responses can differ and still be considered identical.

- *small_response_diff_threshold* : The percentage by which 2 small responses can differ and still be considered identical.



### Examples


#### Ignoring this check

```json
{
    "checks": {
        "information_disclosure/source_code_disclosure": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="high-severity">HIGH</span>**
- OWASP: **[A05:2021](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)**

- CWE
  - **200**
  - **219**
  - **527**
  - **538**
  - **540**
  - **541**
  - **552**
  - **664**
  - **668**




### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N/E:H/RL:O/RC:C**
- CVSS_SCORE: **7.2**

## References

https://www.zaproxy.org/docs/alerts/41/
