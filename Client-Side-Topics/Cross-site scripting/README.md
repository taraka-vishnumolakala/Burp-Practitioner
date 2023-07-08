
# Cross-site scripting (XSS)

## What is an XSS ?
An XSS (Cross-Site Scripting) is a security flaw in a website or web application that lets attackers:
- Bypass security measures meant to separate different websites. 
- Enables attackers to pretend to be a legitimate user and carry out actions on the vulnerable website, just like the victim user would be able to do.

## How does XSS work ?
- XSS works by manipulating a vulnerable website, causing it to deliver malicious JavaScript code to users.
- When the victim's browser executes this malicious code, the attacker gains the ability to fully compromise the user's interaction with the application.
- By exploiting XSS, the attacker can manipulate and control the victim's actions within the compromised application, potentially leading to data theft or unauthorized activities.

## What can XSS be used for ?
- Impersonation or masquerading as the victim user.
- Performing unauthorized actions on the vulnerable website.
- Stealing login credentials, session tokens, or other user data.
- Injecting trojan functionality into the website.

## What is same origin policy ?
The Same Origin Policy is a web browser security feature that restricts scripts from one origin accessing or manipulating data from another origin. It ensures interactions are limited to the same origin, enhancing web application security by mitigating XSS and CSRF risks. Implementation can be achieved through:
- 1. Server-Side Controls:
    - By setting appropriate response headers, such as the **`Access-Control-Allow-Origin`** header, on the server side, you can specify the allowed origins that are permitted to access the server's resources.
    - The **`Access-Control-Allow-Origin`** header specifies the origin or origins that are allowed to make cross-origin requests to the server. For example, you can set it to a specific origin like **`'http://example.com'`** or allow all origins using the wildcard **`'*'`** (less secure).
    - This server-side configuration helps enforce cross-origin restrictions, ensuring that only specified origins are allowed to make requests to the server.
2. Cross-Origin Resource Sharing (CORS):
    - CORS is a mechanism that defines access permissions for cross-origin requests at the server level.
    - By configuring CORS headers on the server, you can specify the allowed origins, HTTP methods, and headers for cross-origin requests.
    - The server responds with appropriate CORS headers, such as `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, and `Access-Control-Allow-Headers`, indicating which origins, methods, and headers are permitted.
    - CORS allows the server to define fine-grained access controls for cross-origin requests, ensuring compliance with the Same Origin Policy.
3. Proxy Servers:
    - Proxy servers can be used to route requests from a web application through the server, making the requests appear as if they originate from the same origin as the application.
    - The proxy server acts as an intermediary, forwarding requests from the application to external APIs or resources on behalf of the application.
    - By routing requests through the proxy server, the requests are not subject to the Same Origin Policy restrictions enforced by the browser.
    - This approach enables the application to interact with cross-origin resources while maintaining the appearance of requests originating from the same origin.
4. JSONP (JSON with Padding):
    - JSONP is a technique used to enable cross-origin requests by wrapping the server's response in a callback function.
    - Instead of making XMLHttpRequests, JSONP requests are made by dynamically adding a `<script>` tag to the page, with the source URL containing the callback function name as a parameter.
    - The server responds with the requested data wrapped in the specified callback function, which is then executed in the browser.
    - However, JSONP has security implications as it requires the server to trust and properly sanitize the callback function name provided in the request URL. It is considered less secure compared to other CORS-based approaches.

### Node.js implementations of CORS:

#### Insecure Example (Allowing all origins):
```javascript
const express = require('express');
const app = express();

app.get('/data', (req, res) => {
  res.header('Access-Control-Allow-Origin', '*');
  // Handle data request
});

app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```

#### Secure Example (Allowing specific origins):
```javascript
const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors({
  origin: ['http://example.com', 'http://localhost:3000'],
}));

app.get('/data', (req, res) => {
  // Handle data request
});

app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```
In the secure example, the `cors` middleware is used to allow requests only from specified origins, enhancing the Same Origin Policy enforcement.

## Testing for XSS
- The `alert()` function has been commonly used as a payload to demonstrate XSS vulnerabilities in web applications.
- However, in recent versions of **Chrome (v92 onwards from July 20th, 2021)**, cross-origin **`iframes`** are prevented from calling **`alert()`**.
- As an alternative, you can try using the **`print()`** function to demonstrate the XSS vulnerability.
- For more detailed information on using **`print()`** instead of **`alert()`**, you can refer to the article titled [alert() is dead, long live print()](https://portswigger.net/research/alert-is-dead-long-live-print).

## Types of XSS attacks

### Reflected XSS:
- Occurs when an application includes data from an HTTP request into the immediate response in an unsafe manner.
- The injected script is reflected back to the user as part of the response.
- Example: **`https://www.insecure-website.com/status?message=<script>/*malicious code here*/</script>`**

### Stored XSS:
- Also known as persistent XSS or second-order XSS.
- Data from an external source is stored by the application and later included in its HTTP responses in an unsafe way.
- Common examples include comments on a blog post, user names, contact details, or a marketing application displaying social media posts.

### DOM-based XSS:
- Occurs when an application's client-side JavaScript processes data from an untrusted source in an unsafe manner, typically by writing the malicious data back to the DOM (Document Object Model).

For more detailed information on each type, you can refer to the corresponding files:
- [Reflected XSS](Reflected%20XSS.md)
- [Stored XSS](Stored%20XSS.md)
- [DOM-based XSS](DOM%20XSS.md)

## How to find and test for XSS
- Manual testing for XSS involves the following steps:
    1. Submitting unique input into every entry point in the application.
    2. Identifying every location where the submitted input is returned in the HTTP responses.
    3. Testing each location individually to determine if carefully crafted input can be used to execute arbitrary JavaScript.
- Exploiting cross-site scripting vulnerabilities involves taking advantage of insecure input handling by injecting malicious scripts that are executed by the victim's browser.  
- Understanding XSS contexts is crucial in crafting effective payloads. XSS contexts refer to the different ways in which user input is processed and rendered by the application, affecting the behavior of potential XSS attacks.

For more detailed information on exploiting XSS vulnerabilities and understanding XSS contexts, you can refer to the following resources:
- [Exploiting Cross-Site Scripting Vulnerabilities](Exploiting%20Cross-Site%20Scripting%20Vulnerabilities.md)
- [XSS Contexts](XSS%20Contexts.md)

## Dangling markup injection
- A technique that can be used to capture data cross-domain in situations where full cross-site scripting is not possible, due to input filters or other defenses.
- It can often be exploited to capture sensistive information that is visible to other users, including CSRF tokens 
- More on [Dangling Markup Injection](Dangling%20Markup%20Injection.md)

## Content Security Policy
 - A browser mechanism aimed at mitigating the impact of cross-site scripting and **some other vulnerabilities**
 - More on [Content Security Policy](Content%20%Security%20%Policy.md)

## Preventing XSS Attacks

## Cheat Sheet
- [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
