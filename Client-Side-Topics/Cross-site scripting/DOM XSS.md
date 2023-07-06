DOM XSS typically occurs when data from attcker-controllable source is passed to a sink that supports dynamic code execution, such as **`eval()`** or **`innerHTML`**

| #   | Lab Name                                                                            | Level        | Type |
| --- | ----------------------------------------------------------------------------------- | ------------ | ---- |
| 1 ✅  | [DOM XSS in document.write sink using source location.search](DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search.md)                         | APPRENTICE   | DOM  |
| 2 ✅  | [DOM XSS in document.write sink using source location.search inside a select element](DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search%20inside%20a%20select%20element.md) | PRACTITIONER | DOM  |
| 3 ✅  | [DOM XSS in innerHTML sink using source location.search](DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search.md)                              | APPRENTICE   | DOM  |
| 4 ✅  | [DOM XSS in jQuery anchor href attribute sink using location.search source](DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source.md)           | APPRENTICE   | DOM  |
| 5 ✅  | [DOM XSS in jQuery selector sink using a hashchange event](DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event.md)                            | APPRENTICE   | DOM  |
| 6   | DOM XSS in AngularJS expression with angle brackets and doubt quotes HTML-encoded   | PRACTITIONER | DOM  |
| 7   | Reflected DOM XSS                                                                   | PRACTITIONER | DOM  |
| 8   | Stored DOM XSS                                                                      | PRACTITIONER | DOM  | 

## Testing for DOM XSS

### Testing HTML sinks
- To start with we need to find HTML sink by placing a random aplhanumeric string values into the source and identify where it is being reflected. 
- For each location where our string appeared we need to find the context and based on the context we need to develop our xss payload. 

> 🗒️ **NOTE**
> Browsers behave differently with regards to URL encoding. Chrome, Firefox and Safari will URL-encode `location.search` and `location.hash`, while IE11 and Microsoft Edge will not URL-encode these sources. 
> **If your data gets URL encoded before processing, then an XSS attack is unlikely to work.**

### Testing for JavaScript execution sinks
- Testing for a JavaScript execution sink for DOM based XSS is difficult. As our input doesn't necessarily appear anywhere within the DOM, we can't search for it.
- Instead we need to use a JavaScript debugger to determine whether and how our input is sent to a sink. 
- For each potential source, we first need to find cases within the page's JavaScript code where the source is being referenced. 
- We need to use JavaScript debugger to add break point and follow how the source is being used. 
- If we find a sink that is originated from our source, we need to refine our input to see if we can deliver a successful XSS attack. 

>🗒️ **NOTE**
**Testing for DOM XSS in the wild is a tedious process, often requiring to manually crawl through complex, minified JavaScript. We can use burp's built-in DOM Invader extension, which automates a lot of the hard work for us.**

## Exploiting DOM XSS with different sources and sinks
- In practice different sources and sinks have differing properties that can affect exploitability and determine what techniques are necessary. 
- There might also be additional validations or other processing of data that must be taken into consideration when attempting to exploit. 

> 🗒️ **NOTE**
> There are a variety of sinks that are relevant to DOM-based vulnerabilities. Refer the list [here](https://portswigger.net/web-security/cross-site-scripting/dom-based#which-sinks-can-lead-to-dom-xss-vulnerabilities)

- **`document.write`** sink works with **`script`** elements, so we can use a simple payload such as:
```JavaScript
document.write('... <script>alert(document.domain)</script> ...');
```

- **`innerHTML`** sink doesn't accept **`script`** elements on any modern browser, nor will **`svg onload`** events fire. We need to use alternative elements like **`img`** or **`iframe`** in conjunction with event handlers such as **`onload`** and **`onerror`**
- An example payload would look like: **`element.innerHTML='... <img src=1 onerror=alert(document.domain)> ...'`**

### Sources and sinks in third party dependencies
- Web applications are often built using third party libraries and frameworks could also be potential sources and sinks for DOM XSS.
#### DOM XSS in jQuery
- If an user controlled source like a parameter in URL is passed to a jQuery function like **`attr()`**, we can essentially execute our JavaScript causing an XSS. 
- A good example of payload that can be sent to **`attr()`** could be **`javascript:alert(document.domain)`**
- Another potential sink to look our for is **`$()`** jQuery's selector function, which can be used to inject malicious objects into the DOM. 
- A popular xss vulnerability that utilized the selector function in conjunction with **`location.hash`** soure for animation or auto scrolling to a particular element on a page. This behavior was implemented using **`hashchange`** event handler. Example code:
```javascript
$(window).on('hashchange', function() { 
	var element = $(location.hash); 
	element[0].scrollIntoView(); 
});
```
- Since **`location.hash`** is user controllable an attacker could inject malicious JavaScript. 
- Most recent versions have patched this vulnerability by preventing attackers from injecting malicious data into a selector when the input begins with a hash character **(`#`)** 
- **To actually exploit this classic vulnerability, we need to find a way to trigger a `hashchange` event without any user interaction. One way to do this is to deliver the exploit via an `iframe`**
```html
<iframe src="https://vulnerable-website.com#" onload="this.src+='<img src=1 onerror=alert(1)>'">
```
