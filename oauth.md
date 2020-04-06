- OAuth2 is used for authorization and not for authentication
- OAuth2 is used for authorization (to communicate between 2 machine to giving or getting grant access)
# Terms in OAuth2 :
- Resource --> The thing to be accessed -the photo-
- Resource Owner --> The person who have access tp the resource -owner photo-
- Resource Server --> The server hosting the protected resources -google drive-
- Client --> The application which needs access to the protected resource -photo printing service-
- Authorization Server --> The server issuing access tokens to the client
- Resouce server and authorization are server usually coupled :
    - Resouce server --> focuss on managing data
    - Authorization server --> focuss on authorizing
- There are actually multiple variation of the OAuth flow and no one is the right way and others are the wrong ways
- Grant Types --> allow you to expose multiple ways for a client to receive an Access Token.
# Grant Types / OAuth Flows
| Grant Type         | Use Case                                                    | 
|:------------------:|:-----------------------------------------------------------:| 
| Authorization Code | 3rd party web, native or 3rd party JS app (with PKCE)       | 
| Implicit           | 3rd party JS app (no longer best practice, use AC with PKCE)| 
| Client Credentials | Application (no user) (exp : microservice)                  | 
| Password           | 1st party (no longer best practice)                         | 

## Few Really Important Grant Types / OAuth Flows :
### Grant Type 1 : Authorization Code
![](img_oauth/1.png?raw=true)
- Actor : 
    1. client --> photo printing server
    2. resource server --> google drive
    3. resource --> photo
1. The user login on client and ask it to get his resource in resource server
2. client ask The auhorization server for resource
3. The auhorization server ask the truth from the user that user asked the client to access his resource
4. If yes, The auhorization server sends the client an authorization token. Its short-lived authorization token.
5. The client uses this authorization token and contact the authorization server (and says yes this is actually me, can you please give and access token? ) to get a second token which is the access token.
6. The authorization server says okay now we have done both side of exchange, now i fully trust you and i give you the access token. Go ahead and use this access token to contact the resource server and access the resource.
7. Now whenever the client want to access the resource, it calls the resource server directly with the token.
8. The Resource server verify with the authorization server if it's true if its correct and all that stuff. It make sure that the token is valid.
    - You can do whatever it wants to make sure the token is valid. It can validate it itself or it can talk to the authorization server and validate it. Whatever it is, it figures out that this is valid token and that it provides the resource to the client.

### Grant Type 2 : Implicit
- Use case :
- Steps : similar with authorization code flow with simplification : 1, 2, 3, 6, 7, 8
- Not as secure as authorization code flow, primarily used with shord-lived access tokens Used with javascript apps

### Grant Type 3 : Client Credentials
- Use case : When the client is well trusted (confidential clients). exp: communication between microservice.
- Steps : 2, 6, 7, 8

### Grant Type 4 : Password
- Use case : 1st party app . Exp : twiiter app to twitter api
- Steps : 1, 2, 6, 7, 8





