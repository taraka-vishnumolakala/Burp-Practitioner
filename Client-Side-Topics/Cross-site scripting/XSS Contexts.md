
## What is an XSS Context ?
- The location within the response where attacker controlled data appears
- This could be within a HTML tag, within a tag attribute which might be quoted, within a javascript string, etc.
- Any input validation or other processing that is being performed on that data by the application

## XSS between HTML tags

- When XSS context is text between HTML tags, we need to introduce some new HTML tags designed to trigger execution of JavaScript.

| #   | Lab Name                                                                                                                                                                      | Level        | XSS Type  |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 1 ✅   | [Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                 | APPRENTICE   | Reflected |
| 2 ✅  | [Stored XSS into HTML context with nothing encoded](Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                                                       | APPRENTICE   | Stored    |
| 3 ✅  | [Reflected XSS into HTML context with most tags and attributes blocked](Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked.md)         | PRACTITIONER | Reflected |
| 4 ✅  | [Reflected XSS into HTML context with all tags blocked except custom ones](Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones.md) | PRACTITIONER | Reflected |
| 5   | [Reflected XSS with event handlers and href attributes blocked](Reflected%20XSS%20with%20event%20handlers%20and%20href%20attributes%20blocked.md)                                                                                                              | EXPERT       | Reflected |
| 6 ✅  | [Reflected XSS with some SVG markup allowed](Reflected%20XSS%20with%20some%20SVG%20markup%20allowed.md)                                                                                                                                  | PRACTITIONER | Reflected |


## XSS in HTML tag attributes

| #    | Lab Name                                                                                                                                                          | Level        | XSS Type  |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------- |
| 1 ✅ | [Reflected XSS into attribute with angle brackets HTML-encoded](Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML-encoded.md)                   | APPRENTICE   | Reflected |
| 2 ✅ | [Store XSS into anchor href attribute with double quotes HTML-encoded](Store%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded.md) | APPRENTICE   | Stored    |
| 3 ✅   | [Reflect XSS in canonical link tag](Reflect%20XSS%20in%20canonical%20link%20tag.md)                                                                               | PRACTITIONER | Reflected | 


## XSS into JavaScript

| #   | Lab Name                                                                                                                  | Level        | XSS Type |
| --- | ------------------------------------------------------------------------------------------------------------------------- | ------------ | -------- |
| 1   | Reflected XSS into a javascript string with single quote and backslash escaped                                            | PRACTITIONER             |          |
| 2   | Reflected XSS into Javascript with angle brackets HTML encoded                                                            | APPRENTICE   |          |
| 3   | Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped       | PRACTITIONER |          |
| 4   | Reflected XSS in a JavaScript URL with some characters blocked                                                            | EXPERT       |          |
| 5   | Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped  | PRACTITIONER |          |
| 6   | Reflected XSS inot a template literal with angle brackets, single, double quotes, backslash and backticks unicode-escaped | PRACTITIONER |          |
