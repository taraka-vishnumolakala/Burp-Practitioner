## file upload vulnerabilities

```markdown
TIP: The `Content-Type` response header provides the information on the type of file being uploaded. If the header isn't being explicitly set by the server (application code) there could be problem. 
```

- **Unrestricted file uploads** 

  - Happen's when there is no protection against the type of files being uploaded. These are very rare to find in the present web applications. Typically there is some level of protection against the files being uploaded.  

- ##### Flawed file type validation

  - Web applications typically depend on `Content-Type` header field to validate the type of file being uploaded. 
  - If the application code is validating `Content-Type` header field only but not the actual content of the file itself that is considered as a flawed file type validation. 
  - For example if the application only allows files with image extensions such as `jpeg, png, jpg, etc.` we can simply change our `Content-Type: image/jpeg` but still have our filename to have it's original name to get it executed. 

- ##### Preventing file execution in user accessible directories

  - Web applications typically disable the ability to execute scripts to directories that are accessible by the end user to upload files. 
  - However, `file-name` field in `multipart/form-data` header may be vulnerable to directory traversal leading us to upload files in other directories that can execute our code. 

  ```markdown
  TIP: When supplying malicious input through burp notice if the characters are being decoded by the server. In such cases url encode the malicious input will result in decoding it to the input we are expecting to pass on to the application. 
  ```

- ##### Insufficient blacklisting of dangerous file types

  - A web application may be filtering the type of files we are uploading by validating the file extensions from the `file-name` being uploaded. 
  - Typically, it is very difficult to cover all the vulnerable file types that can be uploaded by an attacker. 

  ```markdown
  TIP: Apache servers allow developers to configure their web applications at a global level by modifying `/etc/apache2/apache2.conf` file. Alternatively, developers can also add conf files in the individual directories by creating a new `.htaccess` configuration file. 
  For an IIS server we can create a similar conf file by naming it as `web.config`.
  ```

  