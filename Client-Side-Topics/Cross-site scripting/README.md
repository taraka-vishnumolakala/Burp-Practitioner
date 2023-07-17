# Cross-site scripting (XSS)

[Exam Notes](Exam-Notes.md)

## Labs

| #    | Lab Name                                                                                                                                                                                              | Level      | XSS Type  |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------- |
| 1 ✅ | [Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                 | APPRENTICE | Reflected |
| 2 ✅ | [Stored XSS into HTML context with nothing encoded](Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                       | APPRENTICE | Stored    |
| 3 ✅ | [DOM XSS in document.write sink using source location.search](DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search.md)                                   | APPRENTICE | DOM       |
| 4 ✅ | [DOM XSS in innerHTML sink using source location.search](DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search.md)                                             | APPRENTICE | DOM       |
| 5 ✅ | [DOM XSS in jQuery anchor href attribute sink using location.search source](DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source.md) | APPRENTICE | DOM       |
| 6 ✅ | [DOM XSS in jQuery selector sink using a hashchange event](DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event.md)                                     | APPRENTICE | DOM       |
| 7 ✅ | [Reflected XSS into attribute with angle brackets HTML encoded](Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML%20encoded.md)                               | APPRENTICE | Reflected |
| 8 ✅ | [Stored XSS into anchor href attribute with double quotes HTML-encoded](Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded.md)           | APPRENTICE | Stored    |
| 9 ✅ | [Reflected XSS into Javascript with angle brackets HTML encoded](Reflected%20XSS%20into%20Javascript%20with%20angle%20brackets%20HTML%20encoded.md)     | APPRENTICE | Reflected |
| 10 ✅  | [DOM XSS in document.write sink using source location.search inside a select element](DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search%20inside%20a%20select%20element.md)                                                           | PRACTITIONER | DOM       |
| 11 ✅  | [DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded](DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded.md)                                                             | PRACTITIONER | DOM       |
| 12 ✅  | [Reflected DOM XSS](Reflected%20DOM%20XSS.md)                                                                                                                                                                                                                 | PRACTITIONER | Reflected |
| 13 ✅  | [Stored DOM XSS](Stored%20DOM%20XSS.md)                                                                                                                                                                                                                       | PRACTITIONER | Stored    |
| 14 ✅  | [Reflected XSS into HTML context with most tags and attributes blocked](Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked.md)                                                                                         | PRACTITIONER | Reflected |
| 15 ✅  | [Reflected XSS into HTML context with all tags blocked except custom ones](Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones.md)                                                                                 | PRACTITIONER | Reflected |
| 16 ✅  | [Reflected XSS with some SVG markup allowed](Reflected%20XSS%20with%20some%20SVG%20markup%20allowed.md)                                                                                                                                                       | PRACTITIONER | Reflected |
| 17 ✅  | [Reflected XSS in canonical link tag](Reflected%20XSS%20in%20canonical%20link%20tag.md)                                                                                                                                                                       | PRACTITIONER | Reflected |
| 18 ✅  | [Reflected XSS into a JavaScript string with single quote and backslash escaped](Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote%20and%20backslash%20escaped.md)                                                                     | PRACTITIONER | Reflected |
| 19 ✅ | [Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped](Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20escaped.md)      | PRACTITIONER | Reflected |
| 20 ✅ | [Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped](Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped.md)            | PRACTITIONER | Stored    |
| 21 ✅ | [Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped](Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets,%20single,%20double%20quotes,%20backslash%20and%20backticks%20Unicode-escaped.md) | PRACTITIONER | Reflected |
| 22 ✅ | [Exploiting cross-site scripting to steal cookies](Exploiting%20cross-site%20scripting%20to%20steal%20cookies.md)                                                                                                                                             | PRACTITIONER | Exploit   |
| 23 ✅ | [Exploiting cross-site scripting to capture passwords](Exploiting%20cross-site%20scripting%20to%20capture%20passwords.md)                                                                                                                                     | PRACTITIONER | Exploit   |
| 24    | [Exploiting XSS to perform CSRF](Exploiting%20XSS%20to%20perform%20CSRF.md)                                                                                                                                                                                   | PRACTITIONER | Exploit   |
|25|[Reflected XSS with AngularJS sandbox escape without strings](Reflected%20XSS%20with%20AngularJS%20sandbox%20escape%20without%20strings.md)|EXPERT|Reflected|
|26|[Reflected XSS with AngularJS sandbox escape and CSP](Reflected%20XSS%20with%20AngularJS%20sandbox%20escape%20and%20CSP.md)|EXPERT|Reflected|
|27|[Reflected XSS with event handlers and href attributes blocked](Reflected%20XSS%20with%20event%20handlers%20and%20href%20attributes%20blocked.md)|EXPERT|Reflected|
|28|[Reflected XSS in a JavaScript URL with some characters blocked](Reflected%20XSS%20in%20a%20JavaScript%20URL%20with%20some%20characters%20blocked.md)|EXPERT|Reflected|
|29|[Reflected XSS protected by very strict CSP, with dangling markup attack](Reflected%20XSS%20protected%20by%20very%20strict%20CSP,%20with%20dangling%20markup%20attack.md)|EXPERT|Reflected|
|30|[Reflected XSS protected by CSP, with CSP bypass](Reflected%20XSS%20protected%20by%20CSP,%20with%20CSP%20bypass.md)|EXPERT|Reflected|

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
 - More on [Content Security Policy](Content%20Security%20Policy.md)

## Preventing XSS Attacks

## Cheat Sheet
- [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
