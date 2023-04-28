### Objective:
To solve the lab, modify your session token to gain access to the admin panel at `/admin`, then delete the user `carlos`.

### Security Weakness:
1. Server trusts the user controlled header part of the JWT and the algorithm mentioned in the json object. 
2. This leads to bypassing the signature verification by specifying the algorithm as **none**.

### Exploitation Methodology:
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

### Insecure Code:
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

### Secure Code:
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
