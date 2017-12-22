### References:
- http://www.swisspush.org/security/2016/10/17/oauth2-in-depth-introduction-for-enterprises
- https://github.com/exteso/oauth2-step-by-step

### Branches:
- ref-token
- jwt-token

### Test:
```
$ TOKEN=($(curl -s client:secret@localhost:8080/auth/oauth/token -d grant_type=client_credentials | jq -r .access_token))
 
$ curl -s -H "Authorization: Bearer $TOKEN" localhost:9090
{"message":"Hello world!"}
 
$ curl -s -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" -X POST -d "Spring Security" localhost:9090
{"error":"access_denied","error_description":"Access is denied"}
 
$ curl -s -H "Authorization: Bearer $TOKEN" localhost:9090/user
{"error":"access_denied","error_description":"Access is denied"}

```

