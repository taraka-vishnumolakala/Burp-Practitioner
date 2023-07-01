
# Cross-site scripting (XSS)

## Labs

| #    | Lab Name                                                                                                                                                                      | Level        | Type of XSS |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ----------- |
| 1 ✅ | [Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                 | APPRENTICE   | Reflected   |
| 2 ✅ | [Exploiting cross-site scripting to steal cookies](Exploiting%20cross-site%20scripting%20to%20steal%20cookies.md)                                                             | PRACTITIONER |             |
| 3 ✅ | [Exploiting cross-sute scripting to capture passwords](Exploiting%20cross-sute%20scripting%20to%20capture%20passwords.md)                                                     | PRACTITIONER |             |
| 4    | [Exploiting XSS to perform CSRF](Exploiting%20XSS%20to%20perform%20CSRF.md)                                                                                                   | PRACTITIONER |             |
| 5 ✅ | [Stored XSS into HTML context with nothing encoded](Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                       | APPRENTICE   | Stored      |
| 6    | [Reflected XSS into HTML context with most tags and attributes blocked](Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked.md)         | PRACTITIONER | Reflected   |
| 7    | [Reflected XSS into HTML context with all tags blocked except custom ones](Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones.md) | PRACTITIONER | Reflected   |
| 8    | [Reflected XSS with some SVG markup allowed](Reflected%20XSS%20with%20some%20SVG%20markup%20allowed.md)                                                                       | PRACTITIONER | Reflected            |


## What is an XSS ?
- Allows an attacker to circumvent the same origin policy, which is designed to segregate different web sites from each other
- Allows an attacker to masquerade as a victim user and carry out any actions that the user is able to perform on the vulnerable application.

## What can XSS be used for ?
- Impersonate or masquerede as the victim user
- perform any actions the user is able to perform
- capture victims login credentials, session tokens, or any data the user is able to access. 
- inject trojan functionality into the web site.

## What is same origin policy ?

## How does XSS work ?
- works by manipulating a vulnerable web site so that it returns malicious javascript to users
- When malicious code executes inside victims browser, the attacker can fully compromise their interaction with the application

## Proof of concept
- `alert()` function has been the most common payload when trying to identify xss on web applications. 
> 🗒️ **NOTE**
> Unfortunately, there's a slight hitch if you are using chrome. In the latest versions after v92 onward (July 20th, 2021), cross-origin `iframes` are prevented from calling `alert()`. In this scenario, try using `print()` function.
> More relevant information to using `print()` over `alert()` can be found in this article. [alert() is dead, long live print()](https://portswigger.net/research/alert-is-dead-long-live-print)

## Testing

## Types of XSS attacks

### Reflected XSS
- Occurs when an application receives data within a HTTP request and includes that data within the immediate response in an unsafe way.
- Example:
```html
https://www.insecure-website.com/status?message=<script>/*malicious code here*/</script>
```
- [Reflected XSS](Reflected%20XSS.md)

### Stored XSS
- Also known as **persistent xss** or **second order xss**
- Happens when application receives data from external source and includes that data within it's later HTTP responses in an unsafe way.
- Some well known examples where stored xss could happen would be: 
	- comments on a blog post 
	- user names
	- contact details
	- marketing application that displays social media posts
- [Stored XSS](Stored%20XSS.md)

### DOM-based XSS
- Arises when an application contains some client-side javascript that processes data from an untrusted source in an unsafe way, **usually by writing the malicious data back to the DOM**.
- [DOM XSS](DOM%20XSS.md)

## How to find and test for XSS 
- Manually testing for XSS involves submitting some simple but unique input into every entry point in the application
- Idnetifying every location where the submitted input is returned in the HTTP responses
- Testing each location individually to determine whether suitably crafted input can be used to execute arbitrary javascript
- [Exploiting XSS Vulnerabilities](Exploiting%20XSS%20%Vulnerabilities.md)
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
