---
sidebar_position: 21
title: Cyclic query
---

# Cyclic query

## Description

GraphQL allows developers to nest queries and objects. Attackers can abuse this feature by calling a deeply nested query similar to a recursive function and causing a Denial of Service by exhausting CPU, memory, or other resources.

## Remediation

Although the ability to fetch a cyclic query is necessary for some GraphQL application, it is best to always implement security measures to control these cyclic queries:
  -**Set query timeouts**: restrict the time a query is allowed to run.
  -**Set a maximum query depth**: limit the tolerated depth of queries in order to prevent overly deep queries from abusing resources.
  -**Set a maximum query complexity**: limit the complexity of queries to mitigate the abuse of GraphQL resources.
  -**Use server-time-based throttling**: limit the amount of server time a user can consume.
  -**Use query-complexity-based throttling**: limit the total complexity of queries a user can consume.


<details>
    <summary>Apollo</summary>

Although the ability to fetch a cyclic query is necessary for some GraphQL application, it is best to always implement security measures to control these cyclic queries:
-**Set a maximum query depth**: limit the depth of allowed queries in order to prevent overly deep queries from abusing GraphQL resources.

  You can easily limit query depth with the very light [graphql-depth-limit](https://www.npmjs.com/package/graphql-depth-limit) library.

  Add a maximum query depth limit based on your knowledge of the schema and how deep you believe a legitimate query could go.

  ```javascript
  import depthLimit from 'graphql-depth-limit'

  const server = new ApolloServer({
    ...
    validationRules: [depthLimit(5)]
    });
  ```

  Source: <https://escape.tech/blog/9-graphql-security-best-practices/>.


-**Set maximum query complexity**: limit the complexity of allowed queries to prevent overly complex queries from abusing GraphQL resources.

  To do so, add a module to compute the complexity of each query and set a threshold on this complexity so that overly broad requests get canceled.

    For a user-friendly module which requires no schema modification whatsoever, check out the [graphql-validation-complexity](https://github.com/4Catalyzer/graphql-validation-complexity) module.

    ```javascript
    import { createComplexityLimitRule } from 'graphql-validation-complexity';

    const ComplexityLimitRule = createComplexityLimitRule(1000);

    const apolloServer = new ApolloServer({
        ...
        validationRules: [ComplexityLimitRule],
    });
    ```

    For a more customizable module that lets you manually configure the cost of each field/type of your schema, take a look at the [graphql-cost-analysis](https://github.com/pa-bru/graphql-cost-analysis) module.

    This second option is best suited for a more realistic complexity estimator as all fields may not be equal in terms of complexity.

    To learn more about complexity estimation, you can read: [Securing Your GraphQL API from Malicious Queries](https://www.apollographql.com/blog/graphql/security/securing-your-graphql-api-from-malicious-queries/).


    Source: <https://escape.tech/blog/9-graphql-security-best-practices/>.


</details>

<details>
    <summary>Graphene</summary>

With `graphene-django`, it is possible to implement a custom GraphQL backend to limit query complexity, such as this one:
[graphene-django query cost analysis / complexity limits](https://gist.github.com/thibaudlabat/7b86f1a4da34eccfbfa524ca7359e87c).


</details>

## Configuration

> CheckId: `complexity/cyclic_query`


### Examples


#### Ignoring this check

```json
{
    "checks": {
        "complexity/cyclic_query": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="low-severity">LOW</span>**
- OWASP: **[A05:2021](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)**
- PCI DSS: **6.5.8**
- CWE
  - **20**
  - **400**
  - **770**
- WASC: **10**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L/E:H/RL:O/RC:C**
- CVSS_SCORE: **5.1**

## References

https://www.apollographql.com/blog/graphql/security/securing-your-graphql-api-from-malicious-queries/
