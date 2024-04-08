| #   | Lab Name                                                                                                                      | Level      | Type      |     |
| --- | ----------------------------------------------------------------------------------------------------------------------------- | ---------- | --------- | --- |
| 1 ✅ | [Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded) | APPRENTICE | Reflected |     |

## Reflected XSS in different contexts
- The location of the reflected data within the application's response determines the type of payload needed to exploit it and can also impact the vulnerability's impact.
- The context in which the reflected data is used within the application also plays a crucial role in determining the required XSS payload.
- Validation or processing of the submitted data before it is reflected can affect the effectiveness of XSS payloads.

## How to identify and test for reflected XSS
Identify and test for reflected XSS by following these steps:
- Test every entry point in the application, including parameters, data within the URL query string, message body, URL file path, and HTTP headers (note that certain XSS-like behaviors triggered by specific headers may not be exploitable).
- Submit random alphanumeric values as input for each entry point and check if the value is reflected in the application's response.
- Determine the reflection context within the response, such as whether the reflected data appears between HTML tags, within a tag attribute (which may be quoted), or within a JavaScript string.
- Test a candidate payload by using the random value as an XSS payload and searching for it in the response using a tool like Burp Suite's Repeater. Burp Suite will highlight each location where the search term appears.
- Test alternative payloads if the original payload is modified or blocked by the application, exploring different XSS attack vectors that might work.
- Test the XSS attack in a browser if you find a working exploit, to ensure it behaves as intended when executed within the context of a browser.