---
sidebar_position: 39
title: Depth limit
---

# Depth limit

## Description

GraphQL does not limit how deep a query can be.

Since GraphQL schemas are often cyclic graphs, a query like the one below could technically be crafted:

```graphql
query IAmEvil {
  author(id: "abc") {
    posts {
      author {
        posts {
          author {
            posts {
              author {
                # Can go on as deep as the client wants
              }
            }
          }
        }
      }
    }
  }
}
```

This can lead to potential *DoS attacks* or *information leakage*.

## Remediation

Secure your application by preventing clients from abusing query depth.
To do so, add a *Maximum Query Depth* limit based on your knowledge of the schema and how deep you believe a legitimate query could go.
By analyzing the query document's abstract syntax tree (AST), a GraphQL server is able to reject or accept a request based on its depth.

For instance, using graphql-ruby with the max query depth setting set to `3` gives the following result:

```json
{
  "errors": [
    {
      "message": "Query has depth of 6, which exceeds max depth of 3"
    }
  ]
}
```

Since the document's AST is analyzed statically, the query does not even execute, which adds no load on your GraphQL server.

Depth alone is often not enough to cover all abusive queries. For example, a query requesting an enormous amount of nodes on the root will be very expensive but unlikely to be blocked by a query depth analyzer.


<details>
    <summary>Apollo</summary>

This remediation is supported by our [GraphQL Armor](https://github.com/Escape-Technologies/graphql-armor) middleware.

You can also limit the depth of queries with the very light [graphql-depth-limit](https://www.npmjs.com/package/graphql-depth-limit) library.

Check how deep legitimate queries should be and set a maximum depth accordingly.

```javascript
import depthLimit from 'graphql-depth-limit'

const server = new ApolloServer({
  // ...
  validationRules: [depthLimit(5)]
});
```

Source: <https://escape.tech/blog/9-graphql-security-best-practices/>.


</details>

<details>
    <summary>Awsappsync</summary>

For now, AppSync does not allow out-of-the-box query depth limit configuration.

This can however be bypassed by implementing depth limit using Velocity Template Language VTL in Escape's Resolver.
Below is an example of using the Matches regex to determine the length of selectionSetList.
This example enforces a depth limit of 3 and can be added inside of an AppSync resolver function.
```
#set($selectionSetList = $ctx.info.selectionSetList)

#foreach ($item in $selectionSetList)
  #if($item.matches(".*/.*/./."))
    $util.error("Error: Queries with more than 3 levels found. At level - $item")
  #end
#end
#return($ctx.prev.result)
```
Source: https://robertbulmer.medium.com/aws-appsync-rate-and-max-depth-limiting-c536e5ba43d6.


</details>

<details>
    <summary>Graphene</summary>

With `graphene-django`, it is possible to implement a custom GraphQL backend to limit query complexity, such as this one:
[graphene-django query cost analysis / complexity limits](https://gist.github.com/thibaudlabat/7b86f1a4da34eccfbfa524ca7359e87c).


</details>

<details>
    <summary>Graphqlphp</summary>

```php
use GraphQL\GraphQL;
use GraphQL\Validator\Rules\QueryDepth;
use GraphQL\Validator\DocumentValidator;

$rule = new QueryDepth($maxDepth = 10);
DocumentValidator::addRule($rule);

GraphQL::executeQuery(/*...*/);
```

Source: https://github.com/webonyx/graphql-php/blob/master/docs/security.md#limiting-query-depth.


</details>

<details>
    <summary>Graphqlyoga</summary>

This remediation is supported by our [GraphQL Armor](https://github.com/Escape-Technologies/graphql-armor) middleware.

You can also use the standalone [envelop plugin](https://www.npmjs.com/package/@escape.tech/graphql-armor-max-depth).


</details>

<details>
    <summary>Hasura</summary>

Hasura allows for manual query depth limit configuration directly in the security settings:
- Go to Project Console > Security Settings > API Limits.
- Click on "Global".
- Set a depth limit (e.g., 3).


</details>

## Configuration

> CheckId: `complexity/depth_limit`

### Options

- *threshold* : Maximum depth before raising an alert (-1 = infinite).



### Examples


#### Ignoring this check

```json
{
    "checks": {
        "complexity/depth_limit": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="medium-severity">MEDIUM</span>**
- OWASP: **[A05:2021](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)**
- PCI DSS: **6.5.8**
- CWE
  - **20**
  - **400**
  - **664**
  - **770**
- WASC: **10**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L/E:H/RL:O/RC:C**
- CVSS_SCORE: **5.1**

## References

https://www.howtographql.com/advanced/4-security/
