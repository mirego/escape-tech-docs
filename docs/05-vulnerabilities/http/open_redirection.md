---
sidebar_position: 2
title: Open redirection
---

# Open redirection

## Description

Open redirects are part of the OWASP 2010 Top Ten vulnerabilities and occur when an application allows user-supplied input (e.g. http://nottrusted.com) to control an offsite redirect. For example an attacker could supply a user with the following link http://example.com/example.php?url=http://malicious.example.com.
## Remediation

This check looks at user-supplied input in query string parameters and POST data to identify where open redirects might be possible. It is generally an accurate way to find where 301 or 302 redirects could be exploited by spammers or phishing attacks. To avoid the open redirect vulnerability, parameters of the application script/program must be validated before sending 302 HTTP code (redirect) to the client browser. Implement safe redirect functionality that only redirects to relative URL's, or a list of trusted domains


## Configuration

> CheckId: `http/open_redirection`


### Examples


#### Ignoring this check

```json
{
    "checks": {
        "http/open_redirection": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="low-severity">LOW</span>**
- OWASP: **[A05:2021](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)**

- CWE
  - **601**
  - **610**
- WASC: **38**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:N/A:N/E:H/RL:O/RC:C**
- CVSS_SCORE: **4.1**

## References

https://cheatsheetseries.owasp.org/cheatsheets/Unvalidated_Redirects_and_Forwards_Cheat_Sheet.html
