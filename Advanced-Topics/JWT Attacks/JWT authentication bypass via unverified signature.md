
### Objective: 
To solve the lab, modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.
### Security Weakness:
1. The server doesn't verify the signature but instead just decodes the jwt to get the payload information.
2. Only calls **decode()** but doesn't call **verify()**.
### Exploitation Methodology:
1. Login as **wiener** and monitor the requests in burp.
2. Send the **/my-account** request to repeater and make the following changes:
	1. change the GET request to **/admin**
	2. modify the jwt payload from wiener to administrator:
```json
{"iss":"portswigger","sub":"administrator","exp":1682077970}
```
![](6b6c449e6c5129cc27f2ed3778a1726e.png)
3. With the response we see a reference to delete user **carlos**
4. Send another GET request to delete user carlos as admin and we have now completed the lab. 
### Insecure Code:
In this implementation, the `verifyJWT()` function takes a JWT token as input and only decodes it using the `jwt.decode()` function. This is vulnerable to JWT authentication bypass via unverified signature.
```javascript
const jwt = require('jsonwebtoken');

function verifyJWT(token) {
  const decoded = jwt.decode(token);
  return decoded;
}
```
### Secure Code:
In this implementation, the `verifyJWT()` function takes a JWT token and a secret key as input, and verifies the JWT signature using the `jwt.verify()` function. This ensures that the JWT is authentic and has not been tampered with by an attacker.
```javascript
const jwt = require('jsonwebtoken');

function verifyJWT(token, secret) {
  const decoded = jwt.verify(token, secret);
  return decoded;
}
```