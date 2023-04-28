
## Labs
| #    | Lab Name                                                              | Level        | Repeat | 
| ---- | --------------------------------------------------------------------- | ------------ | ------ | 
| 1 ✅ | JWT authentication bypass via unverified signature                    | APPRENTICE   |        | 
| 2 ✅ | JWT authentication bypass via flawed signature verification           | APPRENTICE   |        |      
| 3 ✅ | JWT authentication bypass via weak signing key                        | PRACTITIONER |        |      
| 4 ✅   | JWT authentication bypass via jwk header injection                    | PRACTITIONER |        |      
| 5    | JWT authentication bypass via jku header injection                    | PRACTITIONER |        |      
| 6    | JWT authentication bypass via kid header path traversal               | PRACTITIONER |        |      
| 7    | JWT authentication bypass via algorithm confusion                     | EXPERT       |        |      
| 8    | JWT authentication bypass via algorithm confusion with no exposed key | EXPERT       |        |      


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
![](./Images/0c3b52ffea9b0339ff2d2fae682a4bca.jpg)
3. In other words, a JWT is usually either a JWS or JWE token. When people use the term "JWT", they almost always mean a JWS token. JWEs are very similar, except that the actual contents of the token are encrypted rather than just encoded.


### JWT header parameter injections
1. According to the JWS specification, only the `alg` header parameter is mandatory. 
2. In practice, however, JWT headers (also known as JOSE headers) often contain several other parameters. The following ones are of particular interest to attackers.
	1. **jwk** (JSON Web Key) - Provides an embedded JSON object representing the key.
	2. **jku** (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.
	3. **kid** (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching `kid` parameter.