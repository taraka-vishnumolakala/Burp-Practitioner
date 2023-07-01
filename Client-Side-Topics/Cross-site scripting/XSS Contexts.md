
## What is an XSS Context ?
- The location within the response where attacker controlled data appears
- This could be within a HTML tag, within a tag attribute which might be quoted, within a javascript string, etc.
- Any input validation or other processing that is being performed on that data by the application

## XSS between HTML tags

- When XSS context is text between HTML tags, we need to introduce some new HTML tags designed to trigger execution of JavaScript.

| #   | Lab Name                                                                   | Level        | XSS Type  |
| --- | -------------------------------------------------------------------------- | ------------ | --------- |
| 1   | [Reflected XSS into HTML context with nothing encoded](Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                     | APPRENTICE   | Reflected |
| 2   | [Stored XSS into HTML context with nothing encoded](Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded.md)                        | APPRENTICE   | Stored    |
| 3   | [Reflected XSS into HTML context with most tags and attributes blocked](Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked.md)    | PRACTITIONER | Reflected |
| 4   | [Reflected XSS into HTML context with all tags blocked expect custom ones] | PRACTITIONER | Reflected |
| 5   | [Reflected XSS with event handlers and href attributes blocked ]           | EXPERT       | Reflected |
| 6   | [Reflected XSS with some SVG markup allowed]                               | PRACTITIONER | Reflected | 

## XSS in HTML tag attributes

## XSS into JavaScript

