## Reflected XSS into HTML context with nothing encodedTitle

### Objective:
To solve the lab, perform a cross-site scripting attack that calls the `alert` function.

### Security Weakness:
This lab contains a simple **reflected cross-site scripting** vulnerability in the search functionality.

### Exploitation Methodology:
1. Search feild on the web page is vulnerable to reflected XSS
2. copy paste the following javascript code in the search input and click "search" to solve the lab.
```javascript
<script>alert(1)</script>
```

### Insecure Code:

### Secure Code:
