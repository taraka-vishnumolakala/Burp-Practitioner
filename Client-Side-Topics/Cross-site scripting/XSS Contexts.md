
## What is an XSS Context ?
- XSS context refers to the location within the response where attacker-controlled data appears.
- This context can vary and includes locations such as within an HTML tag, within a tag attribute (which may be quoted), within a JavaScript string, and so on.
- Understanding the XSS context is crucial for crafting effective XSS payloads that can exploit the vulnerability.
- Additionally, it's important to consider any input validation or processing performed by the application on the attacker-controlled data within that context.
- The presence of input validation or other processing can impact the effectiveness and type of XSS payload required to successfully exploit the vulnerability.

## XSS between HTML tags

- When the XSS context is the text between HTML tags, special HTML tags need to be introduced to trigger the execution of JavaScript.
- These new HTML tags are specifically crafted to exploit the XSS vulnerability and execute the desired JavaScript code.
- Examples of such tags include `<script>`, `<img>`, `<iframe>`, `<svg>`, and others, which can be used to inject and execute JavaScript code within the HTML context.
- By injecting JavaScript code using these tags, an attacker can perform malicious actions, such as stealing user data or manipulating the behavior of the website.

| #   | Lab Name                                                                                                                                                                      | Level        | XSS Type  |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 1 ✅   | [Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                 | APPRENTICE   | Reflected |
| 2 ✅  | [Stored XSS into HTML context with nothing encoded](Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                       | APPRENTICE   | Stored    |
| 3 ✅  | [Reflected XSS into HTML context with most tags and attributes blocked](Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked.md)         | PRACTITIONER | Reflected |
| 4 ✅  | [Reflected XSS into HTML context with all tags blocked except custom ones](Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones.md) | PRACTITIONER | Reflected |
| 5   | [Reflected XSS with event handlers and href attributes blocked](Reflected%20XSS%20with%20event%20handlers%20and%20href%20attributes%20blocked.md)                                                                                                              | EXPERT       | Reflected |
| 6 ✅  | [Reflected XSS with some SVG markup allowed](Reflected%20XSS%20with%20some%20SVG%20markup%20allowed.md)                                                                                                                                  | PRACTITIONER | Reflected |


## XSS in HTML tag attributes

- When the XSS context is within an HTML tag attribute value, it may be possible to terminate the attribute value, close the tag, and introduce a new tag.
- In situations where angle brackets are blocked or encoded, it is still possible to exploit the XSS vulnerability by terminating the attribute value and creating a new attribute that allows for a scriptable context, such as an event handler.
- By terminating the attribute value and introducing a new attribute that can execute JavaScript code, an attacker can manipulate the behavior of the website and perform malicious actions.
- Examples of attributes that can be exploited for XSS include `onmouseover`, `onclick`, `onload`, and others, which allow JavaScript code to be executed when the associated event is triggered.

| #    | Lab Name                                                                                                                                                          | Level        | XSS Type  |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 7 ✅ | [Reflected XSS into attribute with angle brackets HTML encoded](Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML%20encoded.md)                   | APPRENTICE   | Reflected |
| 8 ✅ | [Stored XSS into anchor href attribute with double quotes HTML-encoded](Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded.md) | APPRENTICE   | Stored    |
| 9 ✅   | [Reflected XSS in canonical link tag](Reflected%20XSS%20in%20canonical%20link%20tag.md)                                                                               | PRACTITIONER | Reflected | 


## XSS into JavaScript

- When the XSS context is existing JavaScript within the response, various situations can arise, each requiring different techniques to perform an exploit.
- The specific technique needed to exploit the XSS vulnerability depends on the context and how the existing JavaScript is used within the application.
- Some common techniques include manipulating variables, modifying function calls, injecting additional JavaScript code, or leveraging the existing JavaScript logic to execute malicious actions.
- Exploiting XSS within JavaScript requires a deep understanding of the application's JavaScript code and how it interacts with user input.
- Depending on the circumstances, different XSS payloads and injection points may be necessary to successfully execute the desired JavaScript code and achieve the intended malicious actions.

### Terminating the existing script

- In a scenario where our input data is injected into a variable inside a `<script>` tag, we can use a payload to break out of the existing JavaScript and execute our own code.
```html
<script>
	...
	var input = 'controllable data here';
	...
</script>
```
- The following payload can be used:
```html
</script><img src=1 onerror=alert(document.domain)>
```
- **This payload works even though the string is not properly terminated within the injected data.
- **The reason this payload works is because the browser first performs HTML parsing to identify page elements, including blocks of scripts.**
- **Only later does it perform JavaScript parsing to understand and execute the embedded scripts.**
- **The above payload leaves the original script broken, with an unterminated string literal.**
- **However, this doesn't prevent the subsequent script from being parsed and executed in the normal way.**
- **As a result, the injected payload is treated as a separate script and executed, triggering the `onerror` event and executing the JavaScript code within the payload.**

### Breaking out of a JavaScript string

- In cases where breaking out of a JavaScript string and executing JavaScript directly is not possible (as mentioned in the "Terminating the existing script" section), it becomes necessary to repair the script following the XSS context.
- It is important to address any syntax errors that may prevent the entire script from executing.
- Examples of breaking out of a string literal within JavaScript include using specific characters or syntax, such as:
    - **`'-alert(document.domain)-'`**
    - **`';alert(document.domain)//'`**
- If an application doesn't allow breaking out of JavaScript, it may escape single quote characters with a backslash to treat them as literal characters rather than special characters.
- However, if the application fails to escape the backslash character itself, an attacker can use their own backslash character to neutralize the escaping, allowing them to break out of the string.
- For example:
    - **`';alert(document.domain)//`** gets converted to **`\'alert(document.domain)//`**
    - **`\'alert(document.domain)//`** gets converted to **`\\'alert(document.domain)//`**
- If the server uses a whitelist of accepted characters or employs a Web Application Firewall (WAF) to prevent certain requests from reaching the website, bypassing these validations may be necessary.
- One approach to bypassing these validations is by using the **throw** statement with an exception handler. This enables passing arguments to a function without using parentheses.
- For example:
	- **`onerror=alert;throw 1`** assigns the **`alert()`** function to the global exception handler and passes **`1`** as an argument
	- This results in calling the **`alert()`** function with **`1`** as the argument.
- The technique of executing XSS without parentheses and semicolons is further explained in the article titled [XSS without parentheses and semicolons](https://portswigger.net/research/xss-without-parentheses-and-semi-colons).

### Making use of HTML-encoding
- When the XSS context is within an existing JavaScript code that is within a quoted tag attribute, such as an event handler, it is possible to leverage HTML encoding to bypass certain input filters.
- After the browser has parsed the HTML tags and attributes within a response, it performs HTML decoding of tag attribute values before further processing.
- If the server-side application blocks or sanitizes certain characters that are necessary for a successful XSS exploit, you can often bypass the input validation by HTML encoding those characters.
- In the case of blocking or escaping single quote characters, you can use HTML encoding to represent the single quote character as **`&apos;`**.
- For example, if the server blocks or escapes single quote characters, the following payload can be used: **`&apos;-alert(document.domain)-&apos;`**.
- If the input payload is part of an event handler attribute such as `onclick`, the browser HTML decodes the value before interpreting the JavaScript code, resulting in the successful execution of the JavaScript code.

### XSS in JavaScript template literals
- JavaScript template literals are string literals that support embedded JavaScript expressions within them.
- Template literals are enclosed within backticks (**\`\`**)  instead of normal quotation marks.
- Embedded JavaScript expressions are identified within template literals using the **`${...}`** syntax.
- The expressions within **`${...}`** are evaluated dynamically at runtime and can include variables, function calls, or any valid JavaScript code.
- Template literals provide a convenient way to concatenate strings and dynamically generate content.
- When dealing with XSS in JavaScript template literals, it is important to ensure that any user-controlled input is properly sanitized or encoded before being included within the template literal.
- Failure to properly sanitize or validate user-controlled input within template literals can lead to XSS vulnerabilities, allowing an attacker to execute malicious JavaScript code within the context of the template literal.

| #    | Lab Name                                                                                                                                                                                                                                                                                      | Level        | XSS Type  |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 10 ✅ | [Reflected XSS into a javascript string with single quote and backslash escaped](Reflected%20XSS%20into%20a%20javascript%20string%20with%20single%20quote%20and%20backslash%20escaped.md)                                                                                                     | PRACTITIONER | Reflected |
| 11 ✅ | [Reflected XSS into Javascript with angle brackets HTML encoded](Reflected%20XSS%20into%20Javascript%20with%20angle%20brackets%20HTML%20encoded.md)                                                                                                                                           | APPRENTICE   | Reflected |
| 12 ✅ | [Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped](Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20escaped.md)                  | PRACTITIONER | Reflected |
| 13    | [Reflected XSS in a JavaScript URL with some characters blocked](Reflected%20XSS%20in%20a%20JavaScript%20URL%20with%20some%20characters%20blocked.md)                                                                                                                                         | EXPERT       | Reflected |
| 14 ✅ | [Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped](Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped.md)     | PRACTITIONER | Stored    |
| 15 ✅ | [Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks unicode-escaped](Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets,%20single,%20double%20quotes,%20backslash%20and%20backticks%20Unicode-escaped.md) | PRACTITIONER | Reflected          |
