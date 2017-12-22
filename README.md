### References:
- https://docs.spring.io/spring-security/site/docs/4.2.3.RELEASE/reference/htmlsingle/
- https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-security.html
- http://www.swisspush.org/security/2016/10/17/oauth2-in-depth-introduction-for-enterprises
- https://github.com/exteso/oauth2-step-by-step

### Branches:
- ref-token
- jwt-token

### Test:
```
$ keytool -genkeypair -alias jwt -keyalg RSA -dname "CN=jwt, L=hvo, S=hvo, C=ca" -keypass mySecretKey -keystore jwt.jks -storepass mySecretKey
$ mv jwt.jks authorization-server/src/main/resources
 
$ curl -s client:secret@localhost:8080/auth/oauth/token -d grant_type=client_credentials | jq .
{
   "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzY29wZSI6WyJyZXNvdXJjZS1zZXJ2ZXItcmVhZCIsInJlc291cmNlLXNlcnZlci13cml0ZSJdLCJleHAiOjE1MTM5MjUwNDQsImF1dGhvcml0aWVzIjpbIlJPTEVfQVNfUkVBRCJdLCJqdGkiOiIzZWFkNjJhMS0zYjhjLTQyODYtOWY4NC0wNjBkZGEzNTU4YTQiLCJjbGllbnRfaWQiOiJjbGllbnQifQ.cCFphh_6SrzusbHJcjchDgryMKDxVRBeE41XM5KyzxKaDbyA8JT5JWYXSNa4_bnZxintbsBzTqJEB-BqVOomdARUisaOfxxLAV6sdp30OOj8BQqNNUVuGPSc8xfeeaWNp29CyHJwF5TeuytHuaIBlz31z37r8-pIlkOLI6gmDwmhlTD3zQ7A-UAIHmZoK2GV12jdQdRPlzMid9lBMYgO1LLZDoSRihnqyrPT_U9c5IfzMkjJhplGBwnnxWkzM1jEufUi_9xKKeJzSRcaSjfCXjcI1Cnje_fODwtx6zniR4oKroZUvceW-Fybv_-oJzoFfetoSJEezMmPrhm063Opdg",
   "token_type": "bearer",
   "expires_in": 59,
   "scope": "resource-server-read resource-server-write",
   "jti": "3ead62a1-3b8c-4286-9f84-060dda3558a4"
}
$ TOKEN=($(curl -s client:secret@localhost:8080/auth/oauth/token -d grant_type=client_credentials | jq -r .access_token))
 
$ curl -s -H "Authorization: Bearer $TOKEN" localhost:9090
{"message":"Hello world!"}

 
$ curl -s -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" -X POST -d "Spring Security" localhost:9090
{"error":"access_denied","error_description":"Access is denied"}
 
$ curl -s -H "Authorization: Bearer $TOKEN" localhost:9090/user
{"message": "user is: org.springframework.security.oauth2.provider.OAuth2Authentication@51be8919: Principal: client; Credentials: [PROTECTED]; Authenticated: true; Details: remoteAddress=0:0:0:0:0:0:0:1, tokenType=BearertokenValue=<TOKEN>; Granted Authorities: ROLE_AS_READ"}

```

