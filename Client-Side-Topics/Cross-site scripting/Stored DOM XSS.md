## Stored DOM XSS

### Objective:
- This lab demonstrates a stored DOM vulnerability in the blog comment functionality. 
- To solve this lab, exploit this vulnerability to call the `alert()` function.

### Security Weakness:

### Exploitation Methodology:
- Notice that when we click on **View Post** our application makes multiple requests and one of them is a **GET** request call to **/resources/js/loadCommentsWithVulnerableEscapeHtml.js**
- Reading through the JavaScript code we see a sanitization function called **escapeHTML(html)** that takes our input data and replaces angle brackets.
- Notice that the function is using **replace** instead of **replaceAll**. This means the function is only going to replace the first occurence of **<** or **>**.
- We also know that innerHTML doesn't accept script tags. 
- To successfully complete this lab we can use the following payload and add to the comment input. **`<><img src=1 onerror=alert(1)>`**

### Insecure Code:

### Secure Code:
