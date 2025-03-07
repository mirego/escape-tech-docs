---
sidebar_position: 22
title: Automatic Persisted Queries
---

# Automatic Persisted Queries

## Description

The absence of **Automatic Persisted Queries** can cause backend performance problems at scale.

GraphQL clients send queries to Apollo Servers as HTTP requests, including the GraphQL query string.
Depending on your GraphQL schema, the size of a valid query string might be arbitrarily large. As query strings become larger, increased latency and network usage can noticeably degrade client performance.
A persisted query is a query string cached on the server-side, along with its unique identifier (SHA-256 hash of the query). Clients can send this identifier instead of the full query string, drastically reducing request sizes.

To make a query string persist, your GraphQL server must first receive it from a requesting client. Each unique query string must therefore be sent to the server at least once. Once a client has sent a query string to persist, any other client executing that query can benefit from APQ.

## Remediation

To improve network performance for large query strings, enable APQ if your GraphQL server supports it.


<details>
    <summary>Apollo</summary>

Enable Automatic Persisted queries. 
For a complete guide on how to do so, see [Apollo's Automatic Persisted Queries documentation](https://www.apollographql.com/docs/apollo-server/performance/apq/).


</details>

<details>
    <summary>Dgraph</summary>

For a complete guide on the matter, see [dgraph's Persisted Queries documentation](https://dgraph.io/docs/graphql/queries/persistent-queries/).


</details>

<details>
    <summary>Gqlgen</summary>

Enable Automatic persisted Queries
For a complete guide on how to do so, see [gqlgen's Automatic Persisted Queries documentation](https://gqlgen.com/reference/apq/).


</details>

<details>
    <summary>Graphene</summary>

Automatic Persisted Queries are not supported by Graphene alone.

However, if you use Graphene with [Django](https://github.com/graphql-python/graphene-django), the [django-graphql-persist](https://github.com/flavors/django-graphql-persist) library allows you to implement Automatic Persisted Queries.


</details>

<details>
    <summary>Graphqlapiforwp</summary>

Automatic Persisted Queries on graphqlapiforwp are different than with other engines.
Learn more on [graphqlapiforwp's Persisted Query guide](https://graphql-api.com/guides/use/creating-a-persisted-query/). You can also implement custom persisted queries using [WP GraphQL Lock](https://github.com/valu-digital/wp-graphql-lock).


</details>

<details>
    <summary>Ruby</summary>

Add graphql-persisted_queries to your Gemfile `gem 'graphql-persisted_queries'` and add the plugin to your schema class:

```ruby
class GraphqlSchema < GraphQL::Schema
  use GraphQL::PersistedQueries
end
```

Pass the `:extensions` argument as part of a context to all calls of `GraphqlSchema#execute`,
usually it happens in `GraphqlController`, `GraphqlChannel` and tests:

```ruby
GraphqlSchema.execute(
  params[:query],
  variables: ensure_hash(params[:variables]),
  context: {
    extensions: ensure_hash(params[:extensions])
  },
  operation_name: params[:operationName]
)
```

Source: https://github.com/DmitryTsepelev/graphql-ruby-persisted_queries.


</details>

## Configuration

> CheckId: `dos/automatic_persisted_queries`


### Examples


#### Ignoring this check

```json
{
    "checks": {
        "dos/automatic_persisted_queries": {
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
  - **284**
  - **307**
  - **799**
- WASC: **10**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L/E:F/RL:O/RC:C**
- CVSS_SCORE: **4.9**

## References

https://www.apollographql.com/docs/apollo-server/performance/apq/
