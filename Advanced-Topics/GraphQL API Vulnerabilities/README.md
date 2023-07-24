
## Labs

| #  | Lab Name                                                | Level        |
|----|---------------------------------------------------------|--------------|
| 1 ✅  | [Accessing private GraphQL posts](Accessing%20private%20GraphQL%20posts.md)         | APPRENTICE   |
| 2 ✅  | [Accidental exposure of private GraphQL fields](Accidental%20exposure%20of%20private%20GraphQL%20fields.md)   | PRACTITIONER |
| 3 ✅ | [Finding a hidden GraphQL endpoint](Finding%20a%20hidden%20GraphQL%20endpoint.md)      | PRACTITIONER |
| 4 ✅  | [Bypassing GraphQL brute force protections](Bypassing%20GraphQL%20brute%20force%20protections.md) | PRACTITIONER |
| 5 ✅  | [Performing CSRF exploits over GraphQL](Performing%20CSRF%20exploits%20over%20GraphQL.md)     | PRACTITIONER |


## GraphQL Endpoints

### Common endpoint names

GraphQL services often use similar endpoint suffixes. When testing for GraphQL endpoints, you should look to send universal queries to the following locations:
- `/graphql`
- `/api`
- `/api/graphql`
- `/graphql/api`
- `/graphql/graphql`

### Request Methods

- It is always a best practice for production GraphQL endpoints to only accept POST requests that have a content-type **`application/json`**, as this helps protect against CSRF vulnerabilities. 
- However, some endpoints may accept alternative methods, such as GET requests or POST requests that use a content-type of **`x-www-form-urlencoded`**.

## Discovering schema information

- Introspection is a built-in GraphQL function that enables you to query a server information about that schema.
- It helps us to understand how to interact with the GraphQL API. In some cases it might also disclose potentially sensitive data, such as description fields. 
- As example of full introspection query would look like:
```json
query IntrospectionQuery { __schema { queryType { name } mutationType { name } subscriptionType { name } types { ...FullType } directives { name description args { ...InputValue } onOperation #Often needs to be deleted to run query onFragment #Often needs to be deleted to run query onField #Often needs to be deleted to run query } } } fragment FullType on __Type { kind name description fields(includeDeprecated: true) { name description args { ...InputValue } type { ...TypeRef } isDeprecated deprecationReason } inputFields { ...InputValue } interfaces { ...TypeRef } enumValues(includeDeprecated: true) { name description isDeprecated deprecationReason } possibleTypes { ...TypeRef } } fragment InputValue on __InputValue { name description type { ...TypeRef } defaultValue } fragment TypeRef on __Type { kind name ofType { kind name ofType { kind name ofType { kind name } } } }
```

> **🗒️ NOTE**
> If introspection is enabled but the above query doesn't run, try removing the `onOperation`, `onFragment`, and `onField` directives from the query structure. Many endpoints do not accept these directives as part of an introspection query, and you can often have more success with introspection by removing them.

### Visualizing introspection results

- Responses to introspection queries can be full of information, but are often very long and hard to process. 
- We can make use of **GraphQL Visualizer** to view relationships between schema entities more easily. 

## Bypassing GraphQL introspection defences

- If you cannot get introspection queries to run for the API you are testing, try inserting a special character after the `__schema` keyword.
- When developers disable introspection, they could use a regex to exclude the `__schema` keyword in queries. You should try characters like spaces, new lines and commas, as they are ignored by GraphQL but not by flawed regex.
- As such, if the developer has only excluded **`__schema{`**, then the below introspection query would not be excluded.
```json
	{ 
		"query": "query{__schema
		{queryType{name}}}" 
	}
```

- If this doesn't work, try running the probe over an alternative request method, as introspection may only be disabled over POST. Try a GET request, or a POST request with a content-type of **`x-www-form-urlencoded`**.
- The example below shows an introspection probe sent via GET, with URL-encoded parameters.
```basic
	#Introspection probe as GET request 
	GET /graphql?query=query%7B__schema%0A%7BqueryType%7Bname%7D%7D%7D
```

> **🗒️ NOTE**
> If an endpoint will only accept introspection queries over GET and you want to analyze the results of the query using InQL Scanner, you first need to save the query results to a file. You can then load this file into InQL, where it will be parsed as normal.


## GraphQL CSRF

POST requests that use a content type of `application/json` are secure against forgery as long as the content type is validated. In this case, an attacker wouldn't be able to make the victim's browser send this request even if the victim were to visit a malicious site.

However, alternative methods such as GET, or any request that has a content type of `x-www-form-urlencoded`, can be sent by a browser and so may leave users vulnerable to attack if the endpoint accepts these requests. Where this is the case, attackers may be able to craft exploits to send malicious requests to the API.

## Preventing GraphQL attacks

### Protecting against common attack vectors

- If your API is intended for use by the general public then make sure the API's schema doesn't expose unintended fields to the public. 
- Make sure that suggestions are disabled. This prevents attackers from being able to use **`Clairvoyance`** or similar tools to glean information about the underlying schema.  
- Apollo doesn't have the option to disable suggestions. Read this [Github thread](https://github.com/apollographql/apollo-server/issues/3919#issuecomment-836503305) for workaroud. 

### Protecting against brute force attacks

- To protect against brute forcing using **`Aliases`**, limit the query depth of API's queries. "**Query Depth**" refers to the number of levels of nesting within a query. 
- Configure maximum amount of bytes a query can contain. 
- Configure operation limits. "**Operation limits**" enable you to configure the maximum number of unique fields, aliases, and root fields that your API can accept. 
- For information on how to implement these features in Apollo, read this [blog post](https://www.apollographql.com/blog/graphql/security/securing-your-graphql-api-from-malicious-queries/).

### Protecting against CSRF 

- Only accept queries over JSON-encoded POST.
- API validates that content provided matches the supplied content type. 
- API has a secure CSRF token mechanism. 