
## What is an XSS Context ?
- The location within the response where attacker controlled data appears
- This could be within a HTML tag, within a tag attribute which might be quoted, within a javascript string, etc.
- Any input validation or other processing that is being performed on that data by the application

## XSS between HTML tags

- When XSS context is text between HTML tags, we need to introduce some new HTML tags designed to trigger execution of JavaScript.

| #   | Lab Name                                                                                                                                                                      | Level        | XSS Type  |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 1 ✅   | [Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                 | APPRENTICE   | Reflected |
| 2 ✅  | [Stored XSS into HTML context with nothing encoded](Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                       | APPRENTICE   | Stored    |
| 3 ✅  | [Reflected XSS into HTML context with most tags and attributes blocked](Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked.md)         | PRACTITIONER | Reflected |
| 4 ✅  | [Reflected XSS into HTML context with all tags blocked except custom ones](Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones.md) | PRACTITIONER | Reflected |
| 5   | [Reflected XSS with event handlers and href attributes blocked](Reflected%20XSS%20with%20event%20handlers%20and%20href%20attributes%20blocked.md)                                                                                                              | EXPERT       | Reflected |
| 6 ✅  | [Reflected XSS with some SVG markup allowed](Reflected%20XSS%20with%20some%20SVG%20markup%20allowed.md)                                                                                                                                  | PRACTITIONER | Reflected |


## XSS in HTML tag attributes

- When XSS context is into an HTML tag attribute value, we might sometimes be able to terminate the attribute value, close the tag, and introduce new one. 
- In instances where angle brackets are blocked or encoded, we can try to terminate the attribute value, and create a new attribute that creates a scriptable context, such as event handler.

| #    | Lab Name                                                                                                                                                          | Level        | XSS Type  |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 7 ✅ | [Reflected XSS into attribute with angle brackets HTML-encoded](Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML-encoded.md)                   | APPRENTICE   | Reflected |
| 8 ✅ | [Store XSS into anchor href attribute with double quotes HTML-encoded](Store%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded.md) | APPRENTICE   | Stored    |
| 9 ✅   | [Reflect XSS in canonical link tag](Reflect%20XSS%20in%20canonical%20link%20tag.md)                                                                               | PRACTITIONER | Reflected | 


## XSS into JavaScript

- When XSS context is some existing JavaScript within the response, a wide variety of situations can arise, with different techniquest necessary to perform an exploit.

### Terminating the existing script
- Consider an example where our input data is injected to a variable inside **`<script>`** tag.
```html
<script>
	...
	var input = 'controllable data here';
	...
</script>
```
- We can use the following payload to break out of the existing JavaScript and execute our own:
```html
</script><img src=1 onerror=alert(document.domain)>
```
- **How does this payload work when we aren't terminating the string properly?**
	- The reason this works is that the browser first performs HTML parsing to identify the page elements including blocks of scripts
	- Only later it performs JavaScript parsing to understand and execute the embedded scripts
	- **Our above payload ofcourse leaves the original script broken, with an unterminated string literal**
	- **But this doesn't prevent the subsequent script being parsed and executed in the normal way**

### Breaking out of a JavaScript string
- In cases when it is not possible to break out of a string and execute JavaScript directly (as mentioned in Terminating the existing script) it is essential to repair the script following the XSS context.
- This happens because of any syntax errors preventing the whole script from executing. 
- Some examples to break out of string literal:
```javascript
'-alert(docuemnt.domain)-'
';alert(document.domain)//
```
- **What happens if the application doesn't let us break out of JavaScript?**
	- Applications typically escape any single quote characters with a backslash which tells the application to not interpret it as a special character.  
	- But applications in some scenarios fail to escape the backslash character itself. This lets an attacker use their own backslash character to neutralize. 
```javascript
';alert(document.domain)// --> gets converted to \';alert(document.domain)//
\';alert(document.domain)// --> gets converted to \\';alert(document.domain)//
```
- **What if the server uses a whitelist of characters that are accepted?**
	- This could happen when the server validates characters or uses a WAF to prevent requests from reaching the website. 
	- One way of bypassing these validations is using **`throw`** statement with an exception handler. This enables us to pass arguments to a function without using parathesis. 
	- Consider this simple example: **`onerror=alert;throw 1`**. This piece of code assigns **`alert()`** function to the global exception handler and throw statement passes **`1`** to the handler. 
	- This results in calling **`alert()`** function with **`1`** as argument.
	- [XSS without parantheses and semi-colons](https://portswigger.net/research/xss-without-parentheses-and-semi-colons)

### Making use of HTML-encoding
- When XSS context is some existing JavaScript within a quoted tag attribute, such as an event handler, it is possible to make use of HTML encoding to work around some input filters. 
> 🗒️ **NOTE**
> When the browser has parsed out the html tags and attributes within a response, it will perform HTML-decoding of tag attribute values before they are processed any further. 
- If the server side application blocks or sanitizes certain characters that are needed for a successful XSS exploit, you can often bypass the input validation by HTML-encoding those characters. 
- Consider the following example:
	- If the server blocks or escapes single quote characters, we can use the folling example payload **`&apos;-alert(document.domain)-&apos;`**
- If our input payload is part of an event handler such as onclick, browser HTML-decodes the value before the JavaScript is interpreted and results in a successful execution of JavaScript code. 

### XSS in JavaScript template literals
- JavaScript template literals are string literals that allow embedded JavaScript expressions and evaluate them. 
- These are encapsulated using backticks instead of normal quotation marks and embeded expressions are identified using **`${...}`** syntax.
 
| #    | Lab Name                                                                                                                                                                                                                                                                                      | Level        | XSS Type  |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 10 ✅ | [Reflected XSS into a javascript string with single quote and backslash escaped](Reflected%20XSS%20into%20a%20javascript%20string%20with%20single%20quote%20and%20backslash%20escaped.md)                                                                                                     | PRACTITIONER | Reflected |
| 11 ✅ | [Reflected XSS into Javascript with angle brackets HTML encoded](Reflected%20XSS%20into%20Javascript%20with%20angle%20brackets%20HTML%20encoded.md)                                                                                                                                           | APPRENTICE   | Reflected |
| 12 ✅ | [Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped](Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20escape.md)                  | PRACTITIONER | Reflected |
| 13    | [Reflected XSS in a JavaScript URL with some characters blocked](Reflected%20XSS%20in%20a%20JavaScript%20URL%20with%20some%20characters%20blocked.md)                                                                                                                                         | EXPERT       | Reflected |
| 14 ✅ | [Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped](Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped.md)     | PRACTITIONER | Stored    |
| 15 ✅ | [Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks unicode-escaped](Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets,%20single,double%20quotes,%20backslash%20and%20backticks%20unicode-escaped.md) | PRACTITIONER | Reflected          |
