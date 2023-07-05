## Reflected XSS into Javascript with angle brackets HTML encoded

### Objective:
- This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. 
- The reflection occurs inside a JavaScript string. 
- To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the `alert` function.

### Security Weakness:

### Exploitation Methodology:
- From the lab objective we already know that search field is vulnerable to xss
- Let's send a unique alphanumeric value to identify our xss context
- We can see that it's inside a script tag assigned to a variable
![](./Images/d7861445000d29062b052697caa3286e.png)
- When we send our payload to end script tags the application encodes angle brackets as shown below.
![](./Images/ad9593e756c5db4f56f4e5da6f13441f.png)
- We can end our string and inject javascript by using the payload **`';alert(document.domain)//`**
![](./Images/f9ad8e99f9f30f6e258e8587d6fe6ccb.png)

### Insecure Code:

### Secure Code:
