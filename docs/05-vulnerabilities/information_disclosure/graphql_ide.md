---
sidebar_position: 15
title: GraphQL IDE
---

# GraphQL IDE

## Description

A GraphQL IDE provides an interface for users to interact with the Endpoint, but an IDE can also leave room for potential vulnerabilities.

## Remediation

Disable GraphQL IDE, or restrict it.
Head over to your specific engine documentation to know how to do it.


<details>
    <summary>Apollo</summary>

GraphQL Playground is deprecated and disabled by default since Apollo v3.
If you installed it voluntarily with the corresponding plugin, you should consider disabling it to improve security.

If you still use Apollo v2, you can disable GraphQL Playground by either:

* Setting the environment variable `NODE_ENV` to `production`
* Explicitly disabling it:
  ```javascript
  const server = new ApolloServer({
    // ...
    playground: false,
  });
  ```

Source:

* [Apollo v2 Documentation](https://www.apollographql.com/docs/apollo-server/v2/testing/graphql-playground/)
* [Apollo v3 Documentation](https://www.apollographql.com/docs/apollo-server/testing/build-run-queries/#graphql-playground)


</details>

## Configuration

> CheckId: `information_disclosure/graphql_ide`


### Examples


#### Ignoring this check

```json
{
    "checks": {
        "information_disclosure/graphql_ide": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="low-severity">LOW</span>**
- OWASP: **[A05:2021](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)**

- CWE
  - **200**
  - **489**
  - **668**
  - **1295**




### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N/E:P/RL:O/RC:C**
- CVSS_SCORE: **4.8**

## References


