---
sidebar_position: 48
title: Directory traversal
---

# Directory traversal

## Description

Directory traversal occurs when a server allows an attacker to read a file or directories outside of the normal web server directory, and local file inclusion gives the attacker the ability to include an arbitrary local file (from the web server) in the web server's response.

Example: `getProfilePicture(name: '../../../etc/password')` returns the server's `/etc/password` file.

## Remediation

There are multiple ways to prevent directory traversal attacks:
- Avoid using parameters entered directly by the user.
- Set up a file/folder name whitelist system: allow only certain folders and/or types of extensions, thus excluding all others.
- Compartmentalize your data and implement middlewares. These can take the form of an interface to the (potentially external) file system on which the data users may request is stored. If the attached data storage is dedicated to this purpose only and does not contain sensitive data, the risk is limited, even if a user manages to bypass the limitations that this middleware can put in place.
- Restrict access of the GraphQL worker to what is strictly necessary. By restricting as much as possible the files and folders to which the GraphQL worker has access, you reduce the range of files potentially exposed by an attack.
- Take advantage of virtualization. With virtualization, it is possible to have several virtual machines completely isolated from each other. The GraphQL worker can therefore be isolated on its own virtual machine, allowing it access only to the elements absolutely necessary for its proper execution.


## Configuration

> CheckId: `injection/directory_traversal`

### Options

- *skip_objects* : List of object that are to be skipped by the security test.



### Examples


#### Ignoring this check

```json
{
    "checks": {
        "injection/directory_traversal": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="high-severity">HIGH</span>**
- OWASP: **[A03:2021](https://owasp.org/Top10/A03_2021-Injection/)**
- PCI DSS: **6.5.8**
- CWE
  - **22**
  - **23**
  - **24**
  - **25**
  - **26**
  - **27**
  - **28**
  - **29**
  - **30**
  - **31**
  - **32**
  - **33**
  - **34**
  - **35**
  - **36**
  - **37**
  - **38**
  - **39**
  - **40**
- WASC: **33**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N/E:H/RL:O/RC:C**
- CVSS_SCORE: **7.2**

## References

https://escape.tech/blog/file-inclusion-directory-traversal-graphql/
