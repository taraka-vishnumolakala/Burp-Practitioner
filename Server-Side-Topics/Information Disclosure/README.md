
## Labs

| #   | Lab Name                                                                                                         | Level        |     |
| --- | ---------------------------------------------------------------------------------------------------------------- | ------------ | --- |
| 1 ✅ | [Information Disclosure in error messages](Information%20Disclosure%20in%20error%20messages)                     | APPRENTICE   |     |
| 2 ✅ | [Information Disclosure on debug page](Information%20Disclosure%20on%20debug%20page)                             | APPRENTICE   |     |
| 3 ✅ | [Source Code Disclosure via backup files](Source%20Code%20Disclosure%20via%20backup%20files)                     | APPRENTICE   |     |
| 4 ✅ | [Authentication bypass via Information Disclosure](Authentication%20bypass%20via%20Information%20Disclosure)     | APPRENTICE   |     |
| 5 ✅ | [Information Disclosure in version control history](Information%20Disclosure%20in%20version%20control%20history) | PRACTITIONER |     |

## Introduction

Some examples of information disclosure can include:
- Revealing the names of hidden directories, their structure, and their contents via a `robots.txt` file or directory listing
- Providing access to source code files via temporary backups
- Explicitly mentioning database table or column names in error messages
- Unnecessarily exposing highly sensitive information, such as credit card details
- Hard-coding API keys, IP addresses, database credentials, and so on in the source code
- Hinting at the existence or absence of resources, usernames, and so on via subtle differences in application behavior

Common sources of information disclosure:
- developer comments
- error messages
- user account pages
- backup files 
- version control history
- insecure configurations

### writing data to console logs

### Excessive logging
