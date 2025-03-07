---
sidebar_position: 3
title: NoSQL
---

# NoSQL

## Description

A NoSQL injection vulnerability occurs when users can insert (or “inject”) malicious NoSQL code in a legit SQL query that is built from user-submitted input.
A successful NoSQL injection exploit can read sensitive data from the database, modify database data, execute administration operations on the database (such as shutting down the DBMS), recover the content of a given file from the DBMS file system and in some cases issue commands to the operating system.

## Remediation

Primary defenses:
- Use a sanitization library.
- Cast the inputs to the expected type (eg: The username and password are strings so cast the variables to a string).
- Never use `where`, `mapReduce`, or `group` operators with user input: they allow the attacker to inject JavaScript and are therefore much more dangerous than others. For extra safety, set `javascriptEnabled` to false in mongod.conf (if using mongoDB).
- Enforce Least Privilege.


## Configuration

> CheckId: `injection/nosql`

### Options

- *skip_objects* : List of object that are to be skipped by the security test.



### Examples


#### Ignoring this check

```json
{
    "checks": {
        "injection/nosql": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="high-severity">HIGH</span>**
- OWASP: **[A03:2021](https://owasp.org/Top10/A03_2021-Injection/)**
- PCI DSS: **6.5.1**
- CWE
  - **89**
  - **564**
  - **943**
- WASC: **19**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H/E:H/RL:O/RC:C**
- CVSS_SCORE: **9.4**

## References

https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05.6-Testing_for_NoSQL_Injection
