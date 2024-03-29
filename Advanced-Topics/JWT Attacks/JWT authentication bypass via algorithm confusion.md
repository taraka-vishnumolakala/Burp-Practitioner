## JWT authentication bypass via algorithm confusion

### Objective:
To solve the lab, first obtain the server's public key. This is exposed via a standard endpoint. Use this key to sign a modified session token that gives you access to the admin panel at `/admin`, then delete the user `carlos`

### Security Weakness:
1. Server uses a robust RSA key pair to sign and verify tokens. However, due to implementation flaws, this mechanism is vulnerable to algorithm confusion attacks.

### Exploitation Methodology:
##### Part 1 - Obtain the server's public key
1.  In Burp, load the JWT Editor extension from the BApp store.
2.  In the lab, log in to your own account and send the post-login `GET /my-account` request to Burp Repeater.
3.  In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the `administrator` user.
4.  In the browser, go to the standard endpoint `/jwks.json` and observe that the server exposes a JWK Set containing a single public key.
![](./Images/ec08508f85b37e82264695b07233892e.png)
5.  Copy the JWK object from inside the `keys` array. Make sure that you don't accidentally copy any characters from the surrounding array.

##### Part 2 - Generate a malicious signing key
1.  In Burp, go to the **JWT Editor Keys** tab in Burp's main tab bar.
2.  Click **New RSA Key**.
3.  In the dialog, make sure that the **JWK** option is selected, then paste the JWK that you just copied. Click **OK** to save the key.
4.  Right-click on the entry for the key that you just created, then select **Copy Public Key as PEM**.
5.  Use the **Decoder** tab to Base64 encode this PEM key, then copy the resulting string.
6.  Go back to the **JWT Editor Keys** tab in Burp's main tab bar.
7.  Click **New Symmetric Key**. In the dialog, click **Generate** to generate a new key in JWK format. Note that you don't need to select a key size as this will automatically be updated later.
8.  Replace the generated value for the **k** property with a Base64-encoded PEM that you just created.
9.  Save the key.

##### Part 3 - Modify and sign the token
1.  Go back to the `GET /admin` request in Burp Repeater and switch to the extension-generated **JSON Web Token** tab.
2.  In the header of the JWT, change the value of the `alg` parameter to `HS256`.
3.  In the payload, change the value of the `sub` claim to `administrator`.
4.  At the bottom of the tab, click **Sign**, then select the symmetric key that you generated in the previous section.
5.  Make sure that the **Don't modify header** option is selected, then click **OK**. The modified token is now signed using the server's public key as the secret key.
6.  Send the request and observe that you have successfully accessed the admin panel.
7.  In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Insecure Code:

### Secure Code:
