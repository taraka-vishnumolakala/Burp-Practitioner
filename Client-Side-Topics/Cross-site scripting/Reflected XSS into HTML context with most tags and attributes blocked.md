## Reflected XSS into HTML context with most tags and attributes blocked

### Objective:
- This lab contains a reflected XSS vulnerability in the search functionality but uses a web application firewall (WAF) to protect against common XSS vectors.
- To solve the lab, perform a cross-site scripting attack that bypasses the WAF and calls the `print()` function.

> 🗒️ **NOTE**
> Although we can use the payload `<body onbeforeinput=print()>` and invoke the print funtion by typing something on our search input and reflecting the xss on our own browser we can't use it since the lab specifically mentions the solution **must not require any user interaction**.

### Security Weakness:

### Exploitation Methodology:
1. Similar to our other labs, lets start with identifying xss context by injecting a standard XSS vector, such as:
```html
<script>alert('j2lsegp8yvam')</script>
```
2. Observe that this gets blocked by returning a response **Tag is not allowed** 
3. In the next few steps, we'll use use Burp Intruder to test which tags and attributes are being blocked.
4. Open Burp's browser and use the search function in the lab. Send the resulting request to Burp Intruder.
5. In Burp Intruder, in the Positions tab, replace the value of the search term with: `<>`
6. Place the cursor between the angle brackets and click "Add §" twice, to create a payload position. The value of the search term should now look like: `<§§>`
7. Visit the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) and click "Copy tags to clipboard".
8. In Burp Intruder, in the Payloads tab, click "Paste" to paste the list of tags into the payloads list. Click "Start attack".
9. When the attack is finished, review the results. Note that all payloads caused an HTTP 400 response, except for the **body** and **custom tags** payload, which caused a 200 response.
10. Go back to the Positions tab in Burp Intruder and replace your search term with:
    `<body%20=1>`
10. Place the cursor before the `=` character and click "Add §" twice, to create a payload position. The value of the search term should now look like: `<body%20§§=1>`
11. Visit the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) and click "copy events to clipboard".
12. In Burp Intruder, in the Payloads tab, click "Clear" to remove the previous payloads. Then click "Paste" to paste the list of attributes into the payloads list. Click "Start attack".
13. When the attack is finished, review the results. Note that all payloads caused an HTTP 400 response, except for the **onbeforeinput, onbeforetoggle, onratechange, onresize, onscrollend** payloads, which caused a 200 response.
14. Go to the exploit server and paste the following code, replacing `YOUR-LAB-ID` with your lab ID:
```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```
15. Click "Store" and "Deliver exploit to victim".

### Notes
- search input is vulnerable to reflected xss. we can validate this by sending a unique alpha numeric value and check for that in our response.  
- there is a WAF that blocks most tags and only allows a few tags. we can validate this using intruder.
- similarly the WAF also blocks most of the events but only allows a few of them. This can also be validated using intruder.
> 🗒️ **NOTE**
> Browsers when rendering html only allow one **body** tag and merge any events related to inner body tags to the parent body tag. So, in our scenario if we try to inject a body tag with events into our search field, our inner body tag gets removed and the event gets added to the parent body tag.
- Why are we using this exploit code ?
	- To deliver the payload to our victim user and execute it with no interaction we are using **iframe** tag to inject our vulnerable web app with malicious payload and trying to resize the frame **onload** without any interaction 
- How does the exploit code work ?
	- When the user opens exploit server url that contains our malicious html payload, the iframe tries to resize on load and triggers the print function resulting in a successful xss exploit.
 
### Insecure Code:

### Secure Code:
