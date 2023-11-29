A Full fledged management suite suitable for kickstarting any organisation like Universities, Schools, and classical work environments. I will have a build in identity access management highly configurable ( a custom integration of keycloak ). Capable of managing attendance ( advanced geo-verify features ), events ( virtual option within app ), analysis tool and formalities management ( permission letters and other official document things ).



## tools planned to have implementations
| Kind | List |
|-- | -- |
|Backend Services | Spring, Go |
| Frontend | React/Htmx |
|protocols | webRTC, gRPC, p2p, GraphQL, WebSocket |
|messenger | kafka |
|DB | postgress, redis and nosql |
| env | DigitalOcean mostly |


## Finalised Stack
| Service              | Stack/Language            |
| -------------------- | ------------------------- |
| IAM board            | Keycloak                  |
| Core Service         | Spring boot               |
| communication stream | Kafka                     |
| Data Export          | go                        |
| Formalities Service  | Spring / go / python      |
| Frontend Service     | Svelte                    |
| Api's                | REST & ~~gRPC~~ & GraphQL |
| CI/CD                | Jenkins & sonarlint api   |
| mobile               | svelte web app ~~native app~~                           |




## TODO's
- [ ] svelte implementation for the frontend
- [ ] is microservice possible with limited time constrains
- [ ] usablity of Kafka 
- [ ] Workaround on Websockets 
- [ ] Workaround on GraphQL
- [ ] redis needed?
- [ ] Jenkins ( might not be possible to use a cloud suite )
- [ ] Feasablity of local microservice hosting
- [ ] migration of old code base ( Event-Manager )
- [x] use NoSQL datastore for attandance thing



gRPC status
- since no support from the browser side, dropping the gRPC going for websockets and restapis 😥. MIght be usable in complete backend.




### cool stuffs
1. random string background for page  + addition gradient map[https://www.youtube.com/watch?v=oIm6qKTtmH4]




## Idea board
1. Good to a mobile application for attendance verification indivudually ( like triggering biometrics auth and having has out of it ). But as of now we don't have bandwidth for app development, will be having a web app ( verifies attendance via geo-location and unique - in device identifier )
2. unique-mobile identifier:
	1. will be a hash of in device possible parameters and will be used to track the impersonation
3. As of now ( MVP pahse ), WebSocket are used for push notification. ( soon will also be integrated for messaging service too )



## concerns
- user ability to mock geo-location data ( need to block developer option disabled log for apk )
- as of now using web as auth system for students, but dedicated apk will be more secure and robust 
