---
sidebar_position: 46
title: Introspection
---

# Introspection

## Description

GraphQL introspection enables you to query a GraphQL server for information about the underlying schema, including data like types, fields, queries, mutations, and even the field-level descriptions.
It discloses sensitive information that potentially allows an attacker to design malicious operations.

## Remediation

Introspection should primarily be used as a discovery and diagnostic tool when we're in the **development phase** of building out GraphQL APIs.
While it's still possible for bad actors to learn how to write malicious queries by reverse engineering your GraphQL API through a lot of trial and error, **disabling introspection** is a form of security by obscurity.


<details>
    <summary>Apollo</summary>

To disable Introspection, either set `NODE_ENV` to `production` or enforce it :

 ```javascript
 const server = new ApolloServer({
   typeDefs,
   resolvers,
   introspection: false
 });
 ```

 Source: <https://escape.tech/blog/9-graphql-security-best-practices/>


</details>

<details>
    <summary>Ariadne</summary>

When defining the `ariadne.asgi.GraphQL` app make sure to add the flag `introspection=False`


</details>

<details>
    <summary>Awsappsync</summary>

Add ACL rule to prevent GraphQL __schema introspection queries to the API.
This is achieved by blocking any HTTP body that includes the string "__schema".
This would be entered into the Rule JSON editor when creating a web ACL in the AWS WAF Console.

```json
{
    "Name": "Block-Introspection",
    "Priority": 5,
    "Action": {
        "Block": {}
    },
    "VisibilityConfig": {
        "SampledRequestsEnabled": true,
        "CloudWatchMetricsEnabled": true,
        "MetricName": "Block-Introspection"
    },
    "Statement": {
        "ByteMatchStatement": {
            "FieldToMatch": {
                "Body": {}
            },
            "PositionalConstraint": "CONTAINS",
            "SearchString": "__schema",
            "TextTransformations": [
                {
                    "Type": "NONE",
                    "Priority": 0
                }
            ]
        }
    }
}
```

Don't forget to associate the previously created ACL rule with your AppSync API.

For more information refer to :

[AWS AppSync - Developer Guide](https://docs.aws.amazon.com/appsync/latest/devguide/what-is-appsync.html)

[Integrate an AppSync API with AWS WAF](https://docs.aws.amazon.com/appsync/latest/devguide/WAF-Integration.html)

[AWS Web Application Firewall](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)


</details>

<details>
    <summary>Graphene</summary>

When using Graphene, here is how you would disable introspection for your schema.

```python
from graphql import validate, parse
from graphene import ObjectType, Schema, String
from graphene.validation import DisableIntrospection


  class MyQuery(ObjectType):
  name = String(required=True)


  schema = Schema(query=MyQuery)

  # introspection queries will not be executed.

  validation_errors = validate(
    schema=schema.graphql_schema,
    document_ast=parse('THE QUERY'),
    rules=(
      DisableIntrospection,
    )
  )
```

Source: [docs.graphene-python.org](https://docs.graphene-python.org/en/latest/execution/queryvalidation/)


</details>

<details>
    <summary>Graphqlgo</summary>

`graphql-go/graphql` does not allow you to disable Introspection.

However, you can disable it with a custom middleware filtering the keyword `__schema`:

```go

func blockIntrospection(next http.Handler) http.Handler {
  return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    bodyBytes, _ := ioutil.ReadAll(r.Body)
    r.Body.Close()
    r.Body = ioutil.NopCloser(bytes.NewBuffer(bodyBytes))
    body_lower := bytes.ToLower(bodyBytes)
    subslice := "__schema"
    if bytes.Contains(body_lower, []byte(subslice)) {
      no_introspection := "{\"errors\": [{\"message\": \"Introspection is disabled.\"}],\"data\": null}"
      w.Write([]byte(no_introspection))
    } else {
      next.ServeHTTP(w, r)
    }
  })
}

...

func main(){
  ...
  h := handler.New(...) // GraphQL handler

  http.Handle("/graphql", blockIntrospection(h))

}
```


</details>

<details>
    <summary>Graphqljava</summary>

```java
GraphQLSchema schema = GraphQLSchema.newSchema()
.query(StarWarsSchema.queryType)
.fieldVisibility( NoIntrospectionGraphqlFieldVisibility.NO_INTROSPECTION_FIELD_VISIBILITY )
.build();
```

Source: https://www.graphql-java.com/documentation/v11/execution/


</details>

<details>
    <summary>Graphqlphp</summary>

```php
use GraphQL\GraphQL;
use GraphQL\Validator\Rules\DisableIntrospection;
use GraphQL\Validator\DocumentValidator;
DocumentValidator::addRule(new DisableIntrospection());<code>
</code>
```

Source: https://webonyx.github.io/graphql-php/security/#disabling-introspection


</details>

<details>
    <summary>Hasura</summary>

Hasura allows you to control who can run an introspection query.

 To do so:

 - Go to Project Console > Security Settings > Schema Introspection
 - Select a role (e.g., guest)
 - Check "Disabled"

See the [official guide](https://hasura.io/docs/latest/graphql/cloud/security/disable-graphql-introspection/) for more information.


</details>

<details>
    <summary>Ruby</summary>

```ruby
class MySchema < GraphQL::Schema
disable_introspection_entry_points if Rails.env.production?
end
```

Source: https://github.com/rmosolgo/graphql-ruby/blob/master/guides/schema/introspection.md#disabling-introspection


</details>

## Configuration

> CheckId: `information_disclosure/introspection`


### Examples


#### Ignoring this check

```json
{
    "checks": {
        "information_disclosure/introspection": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="info-severityom">INFO</span>**

- PCI DSS: **6.5.8**
- CWE
- WASC: **15**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N/E:F/RL:O/RC:C**
- CVSS_SCORE: **4.9**

## References

https://www.apollographql.com/blog/graphql/security/why-you-should-disable-graphql-introspection-in-production/
