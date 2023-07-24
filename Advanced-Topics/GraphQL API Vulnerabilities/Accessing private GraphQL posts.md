## Accessing private GraphQL posts

### Objective:

- The blog page for this lab contains a hidden blog post that has a secret password. 
- To solve the lab, find the hidden blog post and enter the password.

### Security Weakness:

### Exploitation Methodology:

1. In Burp's browser, access the blog page.    
2. In Burp, go to **Proxy > HTTP history** and notice the following:
    - Blog posts are retrieved using a GraphQL query.
    - In the response to the GraphQL query, each blog post has its own sequential **`id`**.
    - Blog post **`id`** 3 is missing from the list. This indicates that there is a hidden blog post.
3. Use InQL to scan the GraphQL endpoint. Notice that the **`BlogPost`** type has a **`postPassword`** field available.
4. In Burp's browser, select a blog post. Notice that this causes the site to make a GraphQL query that fetches the relevant post data via a direct reference to the post's ID.
5. In the HTTP history, find the relevant GraphQL query. Right-click it and select **Send to Repeater**.
6. In Repeater, modify the **`id`** variable to 3 (that is, the **`id`** of the hidden blog post). Add the **`postPassword`** field to the query.
7. Send the request.
8. Copy the contents of the response's **`postPassword`** field and paste them into the **Submit solution** dialog to solve the lab.

### Insecure Code:

### Secure Code:
