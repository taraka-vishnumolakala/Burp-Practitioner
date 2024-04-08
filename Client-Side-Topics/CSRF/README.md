

## Labs

| #  | Lab Name                                                                 | Level        |
|----|--------------------------------------------------------------------------|--------------|
| 1  | [CSRF vulnerability with no defenses](CSRF%20vulnerability%20with%20no%20defenses)                   | APPRENTICE   |
| 2  | [CSRF where token validation depends on request method](CSRF%20where%20token%20validation%20depends%20on%20request%20method)    | PRACTITIONER |
| 3  | [CSRF where token validation depends on token present](CSRF%20where%20token%20validation%20depends%20on%20token%20being%20present)    | PRACTITIONER |
| 4  | [CSRF where token is not tied to user session](CSRF%20where%20token%20is%20not%20tied%20to%20user%20session)            | PRACTITIONER |
| 5  | [CSRF where token is tied to non-session cookie](CSRF%20where%20token%20is%20tied%20to%20non-session%20cookie)        | PRACTITIONER |
| 6  | [CSRF where token is duplicated in cookie](CSRF%20where%20token%20is%20duplicated%20in%20cookie)                | PRACTITIONER |
| 7  | [SameSite Lax bypass via method override](SameSite%20Lax%20bypass%20via%20method%20override)           | PRACTITIONER |
| 8  | [SameSite Strict bypass via client-side redirect](SameSite%20Strict%20bypass%20via%20client-side%20redirect)       | PRACTITIONER |
| 9  | [SameSite Strict bypass via sibling domain](SameSite%20Strict%20bypass%20via%20sibling%20domain)             | PRACTITIONER |
| 10 | [SameSite Lax bypass via cookie refresh](SameSite%20Lax%20bypass%20via%20cookie%20refresh)               | PRACTITIONER |
| 11 | [CSRF where Referer validation depends on header being present](CSRF%20where%20Referer%20validation%20depends%20on%20header%20being%20present) | PRACTITIONER |
| 12 | [CSRF with broken Referer validation](CSRF%20with%20broken%20Referer%20validation)                      | PRACTITIONER |

## What is CSRF?

- Cross-site request forgery (CSRF) is a web vulnerability that allows an attacker to induce victim users to perform actions that they do not intend to perform. 
- The attacks works by circumventing the same-origin policy, which is designed to prevent different websites from interfering with each other. 
- A successful CSRF attack would result in,
	- change of victims email address on their account
	- attacker gaining full control of victims user account
	- gaining full control of applications data and functionality if the victim has higher privileges

## How does CSRF work ?

There are three conditions that must be in place for a successfull CSRF attack:
- **A relevant action**: An action within the application that the attacker has a reason to induce. Examples: modifying permissions of other users, or modifying user specific data such as changing password's or emails. 
- **Cookie based session handling**: Applicatino solely relies on session cookies to identify the user who has made the requests. 
- **No unpredictable request parameters** Request that performs the actions shouldn't contain any parameters that the attacker can't guess. Example: A function is not vulnerable if an attacker needs to know the value of existing password to change current password. 

## XSS vs CSRF

- The primary difference between XSS and CSRF is that:
	- XSS allows an attacker to execute arbitrary JavaScript within browser of a victim user.
	- CSRF allows an attacker to induce a victim user to perform actions that do not intend to. 
- CSRF can be described as a "one-way" vulnerability, in that while an attacker can induce the victim to issue an HTTP request, they cannot receive the response from that request. 
- Conversely, XSS is "two-way", in that attackers injected script can issue arbitrary requests, read the responses, and exfiltrate data to an external domain of the attacker's choosing. 