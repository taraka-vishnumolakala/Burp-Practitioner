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
	+ jwk and jku header injection
	+ algorithm confusion
+ Vulnerable Code exmaples 
+ Secure coding practices

---
## What is a JWT ?
+ **JSON Web Token (JWTs)** are a standard format for sending cryptographically signed JSON data between two systems to authorize a user.
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
+ JWT is a specification that is extended and implemented by JWS and JWE. 
+ JWS (JSON Web Signature)
	+ Token is **base64** encoded
+ JWE (JSON Web Encryption)
	+ Token is **encrypted**
--
### Example JWT

![decoded-jwt-token]()

---
## Vulnerabilities associated with JWT

---
## Unverified Signature

--
```js[1-6|4]
const jwt = require('jsonwebtoken');

function verifyJWT(token) {
  const decoded = jwt.decode(token);
  return decoded;
}
```

--
### Video Demo

--
```js[1-6|4]
const jwt = require('jsonwebtoken');

function verifyJWT(token, secret) {
  const decoded = jwt.verify(token, secret);
  return decoded;
}
```

---
## Flawed signature verification

--
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
### Video Demo

--
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