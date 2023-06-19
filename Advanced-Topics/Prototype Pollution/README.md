## Labs

| #   | Lab Name                                                                       | Level        |
| --- | ------------------------------------------------------------------------------ | ------------ |
| 1   | [DOM XSS via client-side prototype pollution](DOM%20XSS%20via%20client-side%20prototype%20pollution.md)                                    | PRACTITIONER |
| 2   | DOM XSS via an alternative prototype pollution vector                          | PRACTITIONER |
| 3   | Client-side prototype pollution via flawed sanitization                        | PRACTITIONER |
| 4   | Client-side prototype pollution in third-party libraries                       | PRACTITIONER |
| 5   | Client-side prototype pollution via browser APIs                               | PRACTITIONER |
| 6   | Privilege escalation via server-side prototype pollution                       | PRACTITIONER |
| 7   | Detecting server-side prototype pollution without polluted property reflection | PRACTITIONER |
| 8   | Bypassing flawed input filters for server-side prototype pollution             | PRACTITIONER |
| 9   | Remote code execution via server-side prototype pollution                      | PRACTITIONER |
| 10  | Exfiltrating sensitive data via server-side prototype pollution                | EXPERT       |


## How do prototype pollution vulnerabilities arise ?

These vulnerabilities typically arise when a JS function recursively merges an object containing user-controllable properties into an existing object. Due to the special meaning of `__proto__` in JavaScript context, the merge operation may assign the nested properties to the object's prototype instead of the target object itself. 
```javascript
let username = ""
username.__proto__ // String.prototype 
username.__proto__.__proto__ // Object.prototype 
username.__proto__.__proto__.__proto__ // null
```

## Client-side prototype pollution vulnerabilities

## Server-side prototype pollution vulnerabilities

> **🗒️ Note**
> An easy trap for developers to fall into is forgetting or overlooking the fact that a javascript `for...in` loop iterates over all of an object's enumerable properties, including ones that it has inherited via the prototype chain. This doesn't include built-in properties set by JavaScript's native constructors as these are non-enumerable by default.


References:
https://github.com/dottif/prototype-pollution-static-analysis
https://tldrsec.com/p/tldr-sec-153-postgres-insecure-defaults-sbom-prototype-pollution