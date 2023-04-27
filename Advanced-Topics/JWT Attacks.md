| #   | Lab Name                                                              | Level        | Repeat | Link |
| --- | --------------------------------------------------------------------- | ------------ | ------ | ---- |
| 1 ✅  | JWT authentication bypass via unverified signature                    | APPRENTICE   |        |      |
| 2 ✅  | JWT authentication bypass via flawed signature verification           | APPRENTICE   |        |      |
| 3   | JWT authentication bypass via weak signing key                        | PRACTITIONER |        |      |
| 4   | JWT authentication bypass via jwk header injection                    | PRACTITIONER |        |      |
| 5   | JWT authentication bypass via jku header injection                    | PRACTITIONER |        |      |
| 6   | JWT authentication bypass via kid header path traversal               | PRACTITIONER |        |      |
| 7   | JWT authentication bypass via algorithm confusion                     | EXPERT       |        |      |
| 8   | JWT authentication bypass via algorithm confusion with no exposed key | EXPERT       |        |      |


## Introduction
1. Structure of JWT
	1. header
		1. contains metadata about the token itself
	2. payload
		1. contains actual "claims" about the user
	3. signature
		1. server that issues the token typically generates the signature by hashing **header** and **payload** 
> Note: In most cases, this data can be easily read or modified by anyone with access to the token. Therefore, the security of any JWT-based mechanism is heavily reliant on the cryptographic signature.

2. The process of generating a signature involves a secret signing key. 
3. This whole process provides a way for servers to verify that none of the data within the token has been tampered with since it was issued.

### JWT vs JWE vs JWS
1. The JWT specification is actually very limited. It only defines a format for representing information ("claims") as a JSON object that can be transferred between two parties. 
2. In practice, JWTs aren't really used as a standalone entity. The JWT spec is extended by both the JSON Web Signature (JWS) and JSON Web Encryption (JWE) specifications, which define concrete ways of actually implementing JWTs.
![[0c3b52ffea9b0339ff2d2fae682a4bca.jpg]]
3. In other words, a JWT is usually either a JWS or JWE token. When people use the term "JWT", they almost always mean a JWS token. JWEs are very similar, except that the actual contents of the token are encrypted rather than just encoded.

## Labs

### JWT authentication bypass via unverified signature
#### Objective: 
To solve the lab, modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.
#### Security Weakness:
1. The server doesn't verify the signature but instead just decodes the jwt to get the payload information.
2. Only calls **decode()** but doesn't call **verify()**.
#### Exploitation Methodology:
1. Login as **wiener** and monitor the requests in burp.
2. Send the **/my-account** request to repeater and make the following changes:
	1. change the GET request to **/admin**
	2. modify the jwt payload from wiener to administrator:
```json
{"iss":"portswigger","sub":"administrator","exp":1682077970}
```

![[6b6c449e6c5129cc27f2ed3778a1726e.png]]
3. With the response we see a reference to delete user **carlos**
4. Send another GET request to delete user carlos as admin and we have now completed the lab. 
#### Insecure Code:
In this implementation, the `verifyJWT()` function takes a JWT token as input and only decodes it using the `jwt.decode()` function. This is vulnerable to JWT authentication bypass via unverified signature.
```javascript
const jwt = require('jsonwebtoken');

function verifyJWT(token) {
  const decoded = jwt.decode(token);
  return decoded;
}
```
#### Secure Code:
In this implementation, the `verifyJWT()` function takes a JWT token and a secret key as input, and verifies the JWT signature using the `jwt.verify()` function. This ensures that the JWT is authentic and has not been tampered with by an attacker.
```javascript
const jwt = require('jsonwebtoken');

function verifyJWT(token, secret) {
  const decoded = jwt.verify(token, secret);
  return decoded;
}
```

### JWT authentication bypass via flawed signature verification
#### Objective:
To solve the lab, modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.
#### Security Weakness:
1. Server trusts the user controlled header part of the JWT and the algorithm mentioned in the json object. 
2. This leads to bypassing the signature verification by specifying the algorithm as **none**.
#### Exploitation Methodology:
1. Turn on burp to proxy all requests and login to the portal. 
2. Under *Proxy --> Http history* take a look at the requests and send the */my-account* request to repeater.
3. Make the following modifications:
	1. Change the header and set *alg* to *none*.
	2. Modify the payload and set *sub* to *administrator*
	3. Remove the signature part but **make sure there is a trailing dot after the encoded payload**
	4. Modify the get request from */my-account* to *admin* and send the request. 
```base64
eyJraWQiOiIwM2E5NTA1NC1kNDkzLTQ2NzAtYjkzOS0zY2Y1MTYxNzdiMDAiLCJhbGciOiJub25lIn0%3d.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODI2MTQ4MTh9.
```
```json
{"kid":"03a95054-d493-4670-b939-3cf516177b00","alg":"none"} --> header
{"iss":"portswigger","sub":"administrator","exp":1682614818}
```
4. After receiving a succesful **200 OK** we can see there is a reference link to delete user *carlos*.
5. Send another GET request to the reference link */admin/delete?username=carlos*  using the modified token to complete the lab. 
#### Insecure Code:
In this implementation, the algorithm used to verify the JWT is taken from the JWT header, which can be controlled by an attacker. This can be exploited by an attacker by setting the algorithm to **none** in the JWT header, effectively bypassing signature verification.
```javascript
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
#### Secure Code:
In this implementation, the algorithm used to verify the JWT is specified by the server, rather than trusting the algorithm specified in the JWT header. This effectively prevents attackers from bypassing signature verification by setting the algorithm to **none** in the JWT header.
```javascript
const jwt = require('jsonwebtoken');

function verifyJWT(token) {
  // verify JWT with server-specified algorithm
  jwt.verify(token, process.env.SECRET_KEY, { algorithms: ['HS256'] }, function(err, decoded) {
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

### JWT authentication bypass via weak signing key
#### Objective:
#### Security Weakness:
#### Exploitation Methodology:
#### Insecure Code:
#### Secure Code:
