[Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md) - Apprentice

## Reflected XSS in different contexts
- The location of the reflected data within the applications response determines what type of payload is required to exploit it and might also affect the impact of the vulnerability
- Additionally, if the application performs any validation or other processing on the submitted data before it is reflected, this will generally affect what kind of XSS payload is needed. 

## How to identify and test for reflected XSS
- Testing for every entry point
	- this includes parameters, data within url query string, message body and url file path
	- also includes http headers, although xss-like behavior that can only be triggered via certain HTTP headers may not be exploitable
- Submit random alphanumeric values
	- For each identified entry point submit the random value and see whether the value is reflected in response
- Determine the reflection context
	- For each location within the response determine if it is reflected between html tags, within a tag attribute which might be quoted, within a javascript string, etc.
- Test a candidate payload
	- An efficient way to work is to use the random value as your xss payload and set the value as your search string in repeater's response.
	- burp will highlight each location where the search term appears.
- Test alternative payloads
	- If for some reason the css payload was modified or blocked by the application then you will need to test for alternative payloads that might deliver a working xss attack
- Test the attack in a browser
	- Finally, if you find a working exploit then it's time to test it out from within the browser