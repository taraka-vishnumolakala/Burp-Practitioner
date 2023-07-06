## DOM XSS in jQuery selector sink using a hashchange event

### Objective:
- This lab contains a DOM-based cross-site scripting vulnerability on the home page. 
- It uses jQuery's `$()` selector function to auto-scroll to a given post, whose title is passed via the `location.hash` property.
- To solve the lab, deliver an exploit to the victim that calls the `print()` function in their browser.

### Security Weakness:

### Exploitation Methodology:
- Upon inspecting the page source, we notice that jQuery **`$()`** selector function is being used in conjunction with hashchange event handler. 
- We can create a custom payload to send to our victim, that would look like: **`<iframe src="https://0a43004203ed978488d61d4200df005a.web-security-academy.net/#" onload="this.src+='<img src=1 onerror=print()>'">`**
- Make sure the exploit is working by clicking on **`View exploit`** from our exploit server
- Now we can click on **`Deliver to victim`** to solve the lab.


### Insecure Code:

### Secure Code:
