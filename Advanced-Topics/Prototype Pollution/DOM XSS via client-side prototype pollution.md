## DOM XSS via client-side prototype pollution

### Objective:
1. Find a source that you can use to add arbitrary properties to the global `Object.prototype`.
2. Identify a gadget property that allows you to execute arbitrary JavaScript.
3. Combine these to call `alert()`.

### Security Weakness:

### Exploitation Methodology:
#### Manual Solution
##### Find a prototype pollution source
1. In your browser, try polluting `Object.prototype` by injecting an arbitrary property via the query string:
    `/?__proto__[foo]=bar`
2. Open the browser DevTools panel and go to the **Console** tab.
3. Enter `Object.prototype`.
4. Study the properties of the returned object. Observe that it now has a `foo` property with the value `bar`. You've successfully found a prototype pollution source.

##### Identify a gadget
1. In the browser DevTools panel, go to the **Sources** tab.
2. Study the JavaScript files that are loaded by the target site and look for any DOM [XSS](https://portswigger.net/web-security/cross-site-scripting) sinks.
3. In `searchLogger.js`, notice that if the `config` object has a `transport_url` property, this is used to dynamically append a script to the DOM.
4. Notice that no `transport_url` property is defined for the `config` object. This is a potential gadget for controlling the `src` of the `<script>` element.

##### Craft an exploit
1. Using the prototype pollution source you identified earlier, try injecting an arbitrary `transport_url` property:
    `/?__proto__[transport_url]=foo`
2. In the browser DevTools panel, go to the **Elements** tab and study the HTML content of the page. Observe that a `<script>` element has been rendered on the page, with the `src` attribute `foo`.
3. Modify the payload in the URL to inject an XSS proof-of-concept. For example, you can use a `data:` URL as follows:    
    `/?__proto__[transport_url]=data:,alert(1);`
4. Observe that the `alert(1)` is called and the lab is solved.

#### DOM Invader Solution
1. Open the lab in Burp's built-in browser.
2. [Enable DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader/enabling) and [enable the prototype pollution option](https://portswigger.net/burp/documentation/desktop/tools/dom-invader/prototype-pollution#enabling-prototype-pollution)
3. Open the browser DevTools panel, go to the **DOM Invader** tab, then reload the page.
4. Observe that DOM Invader has identified two prototype pollution vectors in the `search` property i.e. the query string.
5. Click **Scan for gadgets**. A new tab opens in which DOM Invader begins scanning for gadgets using the selected source.
6. When the scan is complete, open the DevTools panel in the same tab as the scan, then go to the **DOM Invader** tab.
7. Observe that DOM Invader has successfully accessed the `script.src` sink via the `transport_url` gadget.
8. Click **Exploit**. DOM Invader automatically generates a proof-of-concept exploit and calls `alert(1)`.

### Insecure Code:

### Secure Code:
