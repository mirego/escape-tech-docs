---
sidebar_position: 31
title: GET based CSRF
---

# GET based CSRF

## Description

CSRF (Cross-Site Request Forgery) occurs when an external website has the ability to make API calls impersonating a user by visiting the website while being authenticated to your API.

Allowing API calls through `GET` requests can lead to CSRF attacks because cookies are added automatically to GET requests made by the browser.

## Remediation

Forbid API calls through `GET` requests to prevent CSRF attacks.


<details>
    <summary>Apollo</summary>

Pass `csrfPrevention: true` to `new ApolloServer()`.

Check out the the [CSRF prevention documentation](https://www.apollographql.com/docs/apollo-server/security/cors#preventing-cross-site-request-forgery-csrf) for the best CSRF prevention techniques.


</details>

<details>
    <summary>Awsappsync</summary>

Make sure that your API does not use Cookie-based authentication.

There are many other ways to authenticate a user with AppSync:
- API Keys
- Amazon Cognito User Pools
- OpenID Connect
- AWS Identity and Access Management (IAM)
- AWS Lamba custom authentication

[AppSync: Authorization and Authentication](https://docs.aws.amazon.com/appsync/latest/devguide/security-authz.html)

Whichever method you use, verify that authentication occurs through headers because authentication headers are not automatically added by the targeted user browser (while Cookies are).

To avoid any risk, you can block every `GET` request and allow only `POST` requests, which are immune to this attack, but it comes at a cost. (see AWS pricing for the corresponding services)

* Block GET requests with AWS API Gateway (prefered method):

Put your AppSync API behind an API Gateway.

[API Gateway Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

You can then configure the API Gateway to act as an HTTP Proxy to your AppSync endpoint and configure it to allow only POST requests.

* Block GET requests with AWS Web Application Firewall:

Add the following Web ACL rule in the AWS WAF Console to block every GET request to the API:

```json
{
  "Name": "block-get",
  "Priority": 0,
  "Statement": {
    "ByteMatchStatement": {
      "SearchString": "GET",
      "FieldToMatch": {
        "Method": {}
      },
      "TextTransformations": [
        {
          "Priority": 0,
          "Type": "NONE"
        }
      ],
      "PositionalConstraint": "EXACTLY"
    }
  },
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "add-headers"
  }
}
```

You can also configure it manually by using the following field values :

If:
  - Field to match = HTTP method
  - Positional constraint = Exactly matches string
  - Search string = GET

Then:
  - Block

See: [AppSync API with AWS WAF](https://docs.aws.amazon.com/appsync/latest/devguide/WAF-Integration.html).

To learn more on AWS WAF, see: [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html).


</details>

## Configuration

> CheckId: `request_forgery/get_based_csrf`


### Examples


#### Ignoring this check

```json
{
    "checks": {
        "request_forgery/get_based_csrf": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="high-severity">HIGH</span>**
- OWASP: **[A05:2021](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)**
- PCI DSS: **6.5.9**
- CWE
  - **345**
  - **346**
  - **352**
  - **693**
- WASC: **9**



### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N/E:H/RL:O/RC:C**
- CVSS_SCORE: **7.2**

## References

https://blog.doyensec.com/2021/05/20/graphql-csrf.html
