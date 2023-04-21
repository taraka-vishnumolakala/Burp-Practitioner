| #   | Lab Name                                                              | Level        | Repeat | Link |
| --- | --------------------------------------------------------------------- | ------------ | ------ | ---- |
| 1   | JWT authentication bypass via unverified signature                    | APPRENTICE   |        |      |
| 2   | JWT authentication bypass via flawed signature verification           | APPRENTICE   |        |      |
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
#### Problem Statement: 
To solve the lab, modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.
#### Issue:
1. The server doesn't verify the signature but instead just decode the jwt to get the payload information.
2. Only calls **decode()** but doesn't call **verify()**.
#### Solution:
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
