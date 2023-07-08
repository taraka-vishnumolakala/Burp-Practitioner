
## Labs

| #   | Lab Name                                                                            | Level        | Type |
| --- | ----------------------------------------------------------------------------------- | ------------ | ---- |
| 1 ✅  | [DOM XSS in document.write sink using source location.search](DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search.md)                         | APPRENTICE   | DOM  |
| 2 ✅  | [DOM XSS in document.write sink using source location.search inside a select element](DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search%20inside%20a%20select%20element.md) | PRACTITIONER | DOM  |
| 3 ✅  | [DOM XSS in innerHTML sink using source location.search](DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search.md)                              | APPRENTICE   | DOM  |
| 4 ✅  | [DOM XSS in jQuery anchor href attribute sink using location.search source](DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source.md)           | APPRENTICE   | DOM  |
| 5 ✅  | [DOM XSS in jQuery selector sink using a hashchange event](DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event.md)                            | APPRENTICE   | DOM  |
| 6 ✅  | [DOM XSS in AngularJS expression with angle brackets and doubt quotes HTML-encoded](DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20doubt%20quotes%20HTML-encoded.md)   | PRACTITIONER | DOM  |
| 7 ✅  | [Reflected DOM XSS](Reflected%20DOM%20XSS.md)                                                                   | PRACTITIONER | DOM  |
| 8 ✅  | [Stored DOM XSS](Stored%20DOM%20XSS.md)                                                                      | PRACTITIONER | DOM  | 


## DOM XSS

- DOM XSS (Cross-Site Scripting) occurs when data from an attacker-controllable source is passed to a sink that supports dynamic code execution within the Document Object Model (DOM).
- The DOM represents the web page structure and provides methods and properties for interacting with it.
- DOM XSS is different from other types of XSS (such as Reflected XSS or Stored XSS) because the payload is not directly injected into the server's response, but rather manipulated and executed within the client-side JavaScript code.
- DOM XSS vulnerabilities typically arise when user-supplied data is directly or indirectly used in sinks like **`eval()`** or **`innerHTML`**.
- The **`eval()`** function is used to dynamically evaluate JavaScript code passed as a string argument.
- The **`innerHTML`** property is used to set or retrieve the HTML content within an element.
- If attacker-controlled data is not properly sanitized or validated before being passed to these sinks, it can lead to the execution of arbitrary JavaScript code within the context of the web page.

## Testing for DOM XSS

### Testing HTML sinks
- Begin by inserting random alphanumeric string values into the input source to identify where they are reflected within the HTML output.
- Analyze each location where the input data appears and determine the specific context surrounding it.
- Consider the HTML tags, attributes, and other relevant factors that define the XSS context.
- Based on the identified context, craft XSS payloads tailored to exploit the vulnerability effectively.
- It's important to be aware that different browsers may exhibit varying behavior regarding URL encoding.
- Chrome, Firefox, and Safari typically URL-encode `location.search` and `location.hash`, while IE11 and Microsoft Edge may not.
- Take into account the specific browser's encoding behavior to ensure the viability of XSS attacks.
- Note that if the data undergoes URL encoding before processing, the likelihood of a successful XSS attack decreases.

> **🗒️ NOTE:** Understanding the behavior of URL encoding in different browsers is crucial for testing and exploiting XSS vulnerabilities effectively. Keep in mind that Chrome, Firefox, and Safari usually URL-encode `location.search` and `location.hash`, while IE11 and Microsoft Edge might not. If the data is URL-encoded before processing, the chances of a successful XSS attack are diminished.

### Testing for JavaScript execution sinks
- Testing for a JavaScript execution sink in DOM-based XSS can be challenging as the input may not visibly appear within the DOM.
- Instead of searching for the input directly, we need to use a JavaScript debugger to identify if and how our input is sent to a sink.
- Start by identifying potential sources within the page's JavaScript code where the input is referenced.
- Utilize a JavaScript debugger to set breakpoints and track how the input is used throughout the code execution.
- By observing the flow of the input, we can identify if it reaches a sink that executes JavaScript dynamically.
- If a sink is found that originates from our input source, further refine the input to determine if a successful XSS attack can be delivered.

> **🗒️ NOTE:** Testing for DOM-based XSS in real-world scenarios can be time-consuming, especially when dealing with complex and minified JavaScript. To streamline the process, tools like Burp's built-in DOM Invader extension can automate much of the manual work involved.

## Exploiting DOM XSS with different sources and sinks
- Different sources and sinks in DOM-based XSS can have varying properties that impact exploitability and require specific techniques for successful exploitation.
- Consider additional validations or data processing that may be in place, as they can affect the feasibility of an exploit.

> **🗒️ NOTE:** Various sinks can lead to DOM-based XSS vulnerabilities. For a comprehensive list of relevant sinks, refer to the [PortSwigger article](https://portswigger.net/web-security/cross-site-scripting/dom-based#which-sinks-can-lead-to-dom-xss-vulnerabilities).

- The **`document.write`** sink works with **`script`** elements, allowing us to use a simple payload. For example:    
```javascript
document.write('... <script>alert(document.domain)</script> ...');
```
- The **`innerHTML`** sink does not accept **`script`** elements on modern browsers, and **`svg onload`** events may not fire. To exploit this sink, we need to use alternative elements like **`img`** or **`iframe`** in combination with event handlers such as **`onload`** and **`onerror`**. 
- An example payload for the **`innerHTML`** sink would be:
```javascript
element.innerHTML = '... <img src=1 onerror=alert(document.domain)> ...';
```

### Sources and sinks in third party dependencies
- Web applications built with third-party libraries and frameworks can also introduce potential sources and sinks for DOM XSS vulnerabilities.
#### DOM XSS in jQuery:**
- If user-controlled data, such as a parameter in a URL, is passed to a jQuery function like **`attr()`**, it can lead to the execution of arbitrary JavaScript and result in XSS.
- An example payload that can be injected into **`attr()`** is **`javascript:alert(document.domain)`**.
- Another important sink to be aware of is the **`$()`** function in jQuery, which serves as a selector function. It can be used to manipulate the DOM and potentially introduce malicious objects.
- An example of an XSS vulnerability in jQuery involved using the selector function **`$()`** in combination with the user-controlled input from **`location.hash`**. This was often used for animations or auto-scrolling to a specific element on a page by utilizing the **`hashchange`** event handler.
- Recent versions of jQuery have patched this vulnerability by preventing the injection of malicious data into a selector when the input begins with a hash character **(`#`)**.
- Exploiting this classic vulnerability requires finding a way to trigger a **`hashchange`** event without any user interaction. One approach is to deliver the exploit through an `<iframe>` tag.
```html
<iframe src="https://vulnerable-website.com#" onload="this.src+='<img src=1 onerror=alert(1)>'">
```

#### DOM XSS in AngularJS:
- When a website uses the **`ng-app`** attribute on the HTML element with AngularJS, any JavaScript code inside double curly braces will be processed by AngularJS. This can introduce potential sources for DOM XSS vulnerabilities.

## DOM XSS combined with reflected and stored data
- Reflected DOM XSS occurs when the server processes data from the request and echoes it into the response. The reflected data may be placed within a JavaScript string literal or a data item within the DOM, such as a form field.
    - If a script on the page processes the reflected data in an unsafe manner and writes it to a vulnerable sink, it can lead to an XSS exploit.
    - An example of this is using the **`eval()`** function to process the reflected data: **`eval('var data = "reflected string"');`**
- Stored DOM XSS occurs when the server receives data from one request, stores it, and includes the data in a later response.  
    - An example of this is setting the **`innerHTML`** property of an element using stored data: **`element.innerHTML = comment.author`**