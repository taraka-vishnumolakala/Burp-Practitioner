## DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

### Objective:
- This lab contains a DOM-based cross-site scripting vulnerability in a AngularJS expression within the search functionality.
- AngularJS is a popular JavaScript library, which scans the contents of HTML nodes containing the `ng-app` attribute (also known as an AngularJS directive). 
- When a directive is added to the HTML code, you can execute JavaScript expressions within double curly braces. This technique is useful when angle brackets are being encoded.
- To solve this lab, perform a cross-site scripting attack that executes an AngularJS expression and calls the `alert` function.

### Security Weakness:

### Exploitation Methodology:
- Let's start with identifying our xss context by entering a alphanumeric value inside search box. 
- View the page source to see where our value is reflected. **Notice the `ng-app` within the body element**. Using this information we can understand that the application is using AngularJS.
- We can use our payload enclosed in curly braces to execute JavaScript. **{{$on.constructor('alert(1)')()}}**

### Insecure Code:

### Secure Code:
