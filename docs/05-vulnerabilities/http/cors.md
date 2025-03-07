---
sidebar_position: 18
title: CORS
---

# CORS

## Description

Attackers can exploit CORS (Cross-Origin Resource Sharing) misconfigurations on the web server to perform CSRF (Cross-Site Request Forgery) attacks and send unauthorized commands from an authenticated user session.

## Remediation

Configure the 'Access-Control-Allow-Origin' HTTP header to a more restrictive set of domains, or remove all CORS headers entirely, to allow the web browser to enforce the Same Origin Policy (SOP) in a more restrictive manner.

If your API is public and used in websites you don't control yourself, you want to allow any request origin and you can safely ignore this alert.

See: [enable-cors.org](https://enable-cors.org/index.html).


<details>
    <summary>Apollo</summary>

Configure the 'Access-Control-Allow-Origin' HTTP header to a more restrictive set of domains, or remove all CORS headers entirely, to allow the web browser to enforce the Same Origin Policy (SOP) in a more restrictive manner.

For instance, with `apollo-server-express`, you can restrain request origin to only a few whitelisted domains:

 ```javascript
 await server.start();

 const corsOptions = {
   origin: ["https://www.your-app.example", "https://studio.apollographql.com"]
 };

 server.applyMiddleware({
   app,
   cors: corsOptions,
   path: "/graphql",
 });
 ```
 Source: <https://www.apollographql.com/docs/apollo-server/security/cors/>.

 If your API is public and used in websites you don't control yourself, you want to allow any request origin and you can safely ignore this alert.


</details>

<details>
    <summary>Awsappsync</summary>

Add CORS headers with the API Gateway.

Put your AppSync API behind an API Gateway and configure it to act as a proxy to your AppSync endpoint (e.g., using the HTTP Proxy feature).

To learn how to do so, see [AWS's API Gateway documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html).

You can then manually enable CORS for each resource (only for one if you created the gateway for a single AppSync endpoint):

API Gateway console > {your api gateway} > Resources > {your created resource} > Actions : Enable CORS


</details>

## Configuration

> CheckId: `http/cors`


### Examples


#### Ignoring this check

```json
{
    "checks": {
        "http/cors": {
            "skip": true
        }
    }
}
```




## Score

- Escape Severity: **<span className="low-severity">LOW</span>**
- OWASP: **[A05:2021](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)**
- PCI DSS: **6.5.9**
- CWE
  - **183**
  - **668**
  - **942**




### CVSS

- CVSS_VECTOR: **CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N/E:H/RL:O/RC:C**
- CVSS_SCORE: **5.1**

## References

https://portswigger.net/web-security/cors
