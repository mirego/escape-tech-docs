---
sidebar_position: 13
title: Tenant isolation
---

# Tenant isolation

## Description

Uses the rules defined by the users to detect same object instances detected by two different users whereas this is prohibited.
According to the rules provided in the configuration file, the same instance or object can be detected by two different users which is prohibited.

## Remediation

When accessing the application via GraphQL, we must validate whether or not the user has access to the requested elements from the schema.
Especially we must implement access control policies **on every path** of the Graph leading considered field or object.

The authorization logic belongs to the business logic layer, and from there it is accessed by GraphQL.
This way, the application can have a single source of truth for authorization, which can then be used for other access points.

Among the several access control policies we can implement in our application, the two most popular ones are **Role-Based Access Control** (RBAC) and **Attribute-Based Access Control** (ABAC).
  - With **Role-Based Access Control**, we grant permissions based on roles, and then assign the roles to the users. For instance, WordPress has an `administrator` role with access to all resources, and the `editor`, `author`, `contributor`, and `subscriber` roles, which each restrict permissions in varying degrees, such as being able to create and publish a blog post, just create it, or just read it.
  - With **Attribute-Based Access Control**, permissions are granted based on metadata that can be assigned to different entities, including users, assets, and environment conditions (such as the time of the day or the visitor's IP address). For instance, in WordPress, the capability `edit_others_posts` is used to validate whether the user can edit other users' posts.

In general terms, ABAC is preferable over RBAC because it allows us to configure permissions with fine-grained control, and the permission is unequivocal in its objective.


<details>
    <summary>Apollo</summary>

See [Apollo's Access Control Documentation](https://www.apollographql.com/docs/apollo-server/security/authentication/#in-resolvers).
For large scale applications, you might want to use a specific package like [](https://github.com/maticzav/graphql-shield) for easy Access Control Management.


</details>

<details>
    <summary>Awsappsync</summary>

Appsync provides several methods for protecting critical information.
- For implementing fine-grained access control, see https://docs.aws.amazon.com/appsync/latest/devguide/security-authz.html#fine-grained-access-control


</details>

<details>
    <summary>Hasura</summary>

See Hasura's detailed documentation for Authorization Management [](https://hasura.io/docs/latest/graphql/core/auth/authorization/permission-rules/)


</details>

## Configuration

> CheckId: `access_control/tenant_isolation`

### Parameters


objects : A list of private `objectNames`. A single instance of this object should not be access by 2 different users. Each object instance is identified by its ID.

```json
{'objects': ['**value**']}
```


scalars : A list of scalar `fieldName`. A specific `scalarValue` of this field should not be access by 2 different users. Each scalar instance is indentified by its value.

```json
{'scalars': {'**value**': ['**value**']}}
```




### Examples


#### Ignoring this check

```json
{
    "checks": {
        "access_control/tenant_isolation": {
            "skip": true
        }
    }
}
```


#### Accessiblity of objects private instances for differents users

```
{
  ... Authentication settings ...
  ... Other configuration settings ...

  "checks": {

    ... Other checks ...

    "access_control/tenant_isolation": {
      "parameters": {
        "objects": [
          "MyVeryPrivateData",            # Record access to object `MyVeryPrivateData`
                                          #  if two different users access the same object
                                          #  (i.e. two different users access the same self bound private data)
                                          #  the an alert will be raised.
        ],
        "scalars": {
          "Post": [
            "createdBy",                  # Record access to field `createdBy` of object `Post`
                                          #  if two different users can access the same scalar value
                                          #  an alert will be raised.
          ]
        }
      }
    }

    ... Other checks ...
  }

  ... Other configuration settings ...
}
```





## Score

- Escape Severity: **<span className="high-severity">HIGH</span>**
- OWASP: **[A01:2021](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)**
- PCI DSS: **6.5.8**
- CWE
  - **200**
  - **201**
  - **284**
  - **668**
  - **1198**
  - **1212**
  - **1220**
- WASC: **22**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N/E:H/RL:O/RC:C**
- CVSS_SCORE: **7.2**

## References

https://blog.logrocket.com/authorization-access-control-graphql/
