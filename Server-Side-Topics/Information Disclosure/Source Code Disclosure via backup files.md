## Source Code Disclosure via backup files

### Objective:
This lab leaks its source code via backup files in a hidden directory. To solve the lab, identify and submit the database password, which is hard-coded in the leaked source code.

### Security Weakness:

### Exploitation Methodology:
1.  Browse to `/robots.txt` and notice that it reveals the existence of a `/backup` directory. Browse to `/backup` to find the file `ProductTemplate.java.bak`. Alternatively, right-click on the lab in the site map and go to "Engagement tools" > "Discover content". Then, launch a content discovery session to discover the `/backup` directory and its contents.
2.  Browse to `/backup/ProductTemplate.java.bak` to access the source code.
3.  In the source code, notice that the connection builder contains the hard-coded password for a Postgres database.
4.  Go back to the lab, click "Submit solution", and enter the database password to solve the lab.

### Insecure Code:

### Secure Code:
