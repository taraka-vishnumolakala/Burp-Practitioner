---
theme: ../../../.obsidian/plugins/obsidian-advanced-slides/css/sunblind.css

---
# JWT Attacks: Understanding Common Vulnerabilities

---

## Agenda
+ What are JWT's ?
+ Understanding common vulnerabilities
	+ Unverified signature
	+ Flawed signature verification
	+ Weak signing key 
+ Coding Exercise
+ More about JWT vulnerabilities

---
## What is a JWT ?
+ **JSON Web Token (JWTs)** are a standard format for sending cryptographically signed JSON data between two systems to **authorize** a user.
+ They typically contain **claims** that defines what specific actions a user can perform on an application.

--
### Structure of JWT
+ **Header**
	+ Contains meta data about the token itself
+ **Payload**
	+ Contains the actuals **claims** about the user
+ **Signature**
	+ Server that issues the token typically generates the signature by hashing **header** and **payload**

--
### Types of JWT
+ JWT is a specification that is extended and implemented by **JWS** and **JWE**. 
+ JWS (JSON Web Signature)
	+ Token is **base64** encoded
+ JWE (JSON Web Encryption)
	+ Token is **encrypted**
--
### Example JWT

<split even gap="2">
<img style="height:22%;" src="Burp-Practitioner/Advanced-Topics/JWT Attacks/Images/c8d37b98aab9ca856439b9d6a8c8e0e0.png">

<img style="height:22%;" src="Burp-Practitioner/Advanced-Topics/JWT Attacks/Images/5a011ec45ec5c160c00b3c0c580359ff.png">
</split>

---
## Unverified Signature

This type of attack is possible when the signature of a JWT is not properly verified or not verified at all.  

---
## Flawed Signature Verification

Happens in a scenario where the JWT coming from external user is trusted and the header value of the token is taken from the request. 

An attacker can bypass signature verfication by changing the `alg` value in header to `none` and modify the JWT payload.

---
## Weak Signing Key

These type of vulnerabilities occur when a weak signing key is used by the backend server.

If an external malicious actor can successfully brute-force the signing key, then can create and sign their own keys to bypass authorization.

---
## Coding Exercise

---
<h4 class="fragment fade-up"> Unverified Signature </h4>

```js[1-6|4]
const jwt = require('jsonwebtoken');

function verifyJWT(token) {
  const decoded = jwt.decode(token);
  return decoded;
}
```

--
#### Secure coding practice

```js[1-6|4]
const jwt = require('jsonwebtoken');

function verifyJWT(token, secret) {
  const decoded = jwt.verify(token, secret, { algorithms: ['HS256']);
  return decoded;
}
```

---
<h4 class="fragment fade-up"> Flawed signature verification </h4>

```js[1-21|4-6|8-12|4-6,8-12]
const jwt = require('jsonwebtoken');

function verifyJWT(token) {
  // get algorithm from JWT header
  const header = jwt.decode(token, { complete: true }).header;
  const algorithm = header.alg;
  
  // verify JWT with user-supplied algorithm
  const options = {
    algorithms: [algorithm]
  };
  jwt.verify(token, process.env.SECRET_KEY, options, function(err, decoded) {
    if (err) {
      // JWT signature verification failed
      console.error('JWT signature verification failed: ', err);
    } else {
      // JWT signature verification succeeded
      console.log('JWT signature verification succeeded');
    }
  });
}
```

--
#### Secure coding practice
```js[1-14|5]
const jwt = require('jsonwebtoken');

function verifyJWT(token) {
  // verify JWT with server-specified algorithm
  jwt.verify(token, process.env.SECRET_KEY, { algorithms: ['HS256'] }, 
  function(err, decoded) {
    if (err) {
      // JWT signature verification failed
      console.error('JWT signature verification failed: ', err);
    } else {
      // JWT signature verification succeeded
      console.log('JWT signature verification succeeded');
    }
  });
}
```

---
<h4 class="fragment fade-up"> Weak Signing Key </h4>

```js[1-5|3]
const jwt = require('jsonwebtoken');
function signJWT(payload) {
	const token = jwt.sign(payload, 'secret1');
	return token;
}
```

--
#### Secure coding practice
```js[1-10|4-5]
const jwt = require('jsonwebtoken');
const crypto = require('crypto');

// Generate a random secret using a strong algorithm 
const secret = crypto.randomBytes(64).toString('hex');

// Sign the payload using the strong secret 
function signJWT(payload) {
	return jwt.sign(payload, secret); 
}
```

---
## More about JWT vulnerabilities
JWT Attacks: Understanding Common Vulnerabilities
<a style="font-size: 50%;">https://goodleap.atlassian.net/wiki/spaces/SEC/pages/3063808031/JWT+Attacks+Understanding+Common+Vulnerabilities</a>

---
## Other resources
Secure Coding Guidelines 
<a style="font-size: 50%;">https://goodleap.atlassian.net/wiki/spaces/SEC/pages/3063808009/Secure+Coding+Guidelines</a>

---
## Questions