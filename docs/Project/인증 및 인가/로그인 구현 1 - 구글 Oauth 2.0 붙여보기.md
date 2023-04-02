---  
category: Project/인증 및 인가  
share: true  
---  
  
google Oauth2 써서 로그인 구현해보고 싶음  
  
📘 북마크  
[OAuth 2.0을 사용하여 Google API에 액세스하기  |  Authorization  |  Google Developers](https://developers.google.com/identity/protocols/oauth2)  
[모바일 및 데스크톱 앱용 OAuth 2.0  |  Authorization  |  Google Developers](https://developers.google.com/identity/protocols/oauth2/native-app)  
[OAuth 2.0 — OAuth](https://oauth.net/2/)]  
[웹 앱에 Google 로그인 통합  |  Authentication  |  Google Developers](https://developers.google.com/identity/sign-in/web/sign-in)  
[OAuth2 Examples for Node.js](https://www.example-code.com/nodejs/oauth2.asp)  
[OpenID Connect  |  Authentication  |  Google Developers](https://developers.google.com/identity/openid-connect/openid-connect)  
  
  
  
---  
  
  
![Pasted image 20221219172238.png](../../attachments/Pasted%20image%2020221219172238.png)  
  
  
[Google 클라우드 플랫폼](https://console.cloud.google.com/apis/credentials/oauthclient/447695756565-98a7l5k5dl5ut32k0afo4n6f02ji7js8.apps.googleusercontent.com?project=desk-arrangement-project)  
  
   
구글에서 `https://api.cozihouse.net/auth/google/redirect`  
  
  
# 참고 코드  
[NestJS  Access & Refresh Token JWT Auth | by Jake Engel | Medium | JavaScript in Plain English](https://javascript.plainenglish.io/nestjs-implementing-access-refresh-token-jwt-authentication-97a39e448007)  
  
[spring-security-oauth/schema.sql at main · spring-attic/spring-security-oauth · GitHub](https://github.com/spring-attic/spring-security-oauth/blob/main/spring-security-oauth2/src/test/resources/schema.sql)  
```sql  
-- used in tests that use HSQL  
create table oauth_client_details (  
  client_id VARCHAR(256) PRIMARY KEY,  
  resource_ids VARCHAR(256),  
  client_secret VARCHAR(256),  
  scope VARCHAR(256),  
  authorized_grant_types VARCHAR(256),  
  web_server_redirect_uri VARCHAR(256),  
  authorities VARCHAR(256),  
  access_token_validity INTEGER,  
  refresh_token_validity INTEGER,  
  additional_information VARCHAR(4096),  
  autoapprove VARCHAR(256)  
);  
  
create table oauth_client_token (  
  token_id VARCHAR(256),  
  token LONGVARBINARY,  
  authentication_id VARCHAR(256) PRIMARY KEY,  
  user_name VARCHAR(256),  
  client_id VARCHAR(256)  
);  
  
create table oauth_access_token (  
  token_id VARCHAR(256),  
  token LONGVARBINARY,  
  authentication_id VARCHAR(256) PRIMARY KEY,  
  user_name VARCHAR(256),  
  client_id VARCHAR(256),  
  authentication LONGVARBINARY,  
  refresh_token VARCHAR(256)  
);  
  
create table oauth_refresh_token (  
  token_id VARCHAR(256),  
  token LONGVARBINARY,  
  authentication LONGVARBINARY  
);  
  
create table oauth_code (  
  code VARCHAR(256), authentication LONGVARBINARY  
);  
  
create table oauth_approvals (  
	userId VARCHAR(256),  
	clientId VARCHAR(256),  
	scope VARCHAR(256),  
	status VARCHAR(10),  
	expiresAt TIMESTAMP,  
	lastModifiedAt TIMESTAMP  
);  
  
  
-- customized oauth_client_details table  
create table ClientDetails (  
  appId VARCHAR(256) PRIMARY KEY,  
  resourceIds VARCHAR(256),  
  appSecret VARCHAR(256),  
  scope VARCHAR(256),  
  grantTypes VARCHAR(256),  
  redirectUrl VARCHAR(256),  
  authorities VARCHAR(256),  
  access_token_validity INTEGER,  
  refresh_token_validity INTEGER,  
  additionalInformation VARCHAR(4096),  
  autoApproveScopes VARCHAR(256)  
);  
```  
  
  
Google OAuth2 액세스 토큰 - 일반적으로 실시간 데이터베이스에서 읽고 쓰는 기능은 실시간 데이터베이스 규칙 에 따라 결정됩니다. 그러나 서버에서 데이터에 액세스하고 서비스 계정에서 생성된 Google OAuth2 액세스 토큰을 사용하여 해당 서버에 데이터에 대한 전체 읽기 및 쓰기 액세스 권한을 부여할 수 있습니다.  
  
  
![Pasted image 20221220153649.png](../../attachments/Pasted%20image%2020221220153649.png)  
[Diagrams And Movies Of All The OAuth 2.0 Flows | by Takahiko Kawasaki | Medium](https://darutk.medium.com/diagrams-and-movies-of-all-the-oauth-2-0-flows-194f3c3ade85)  
- app - 구글  
1. 유저가 구글로 로그인 하겠다고 한다.  
2. 구글 Authorization endpoint로 인증 request 보냄  
3. 구글이 로그인 페이지를 app에게 보낸다. (authorization page 리턴)  
4. authorization 유저한테 보여주고 동의 요청함  
   (app에서 이런거 요구한다 ~ 동의하고 로그인 할래~?) → ㅇㅋ함: Authorization Endpoint 로 값 전달  
5. 구글에서 잠깐 살아있는 authorization code 발행함  
6. app에서 살아잇는 그 코드를 token endpoint 에다가 제출함  
7. 구글이 access token 을  app에다가 줌  
  
- app - server (WEB API call)  
A. app이 access token 이랑 requests Resource 를 app 서버에 제출함  
B. DB 에서 구글 서버에게 access token 에 대해서 물어봄  
C. 구글 서버에서 해당 access token 에 대한 정보를 반환해줌  
D. WEB API 찌름  
```  
  
     +----------+  
     | Resource |  
     |  Owner   |  
     |          |  
     +----------+  
          ^  
          |  
         (B)  
     +----|-----+          Client Identifier     +---------------+  
     |         -+----(A)-- & Redirection URI --->|               |  
     |  User-   |                                | Authorization |  
     |  Agent  -|----(B)-- User authenticates -->|     Server    |  
     |          |                                |               |  
     |          |<---(C)--- Redirection URI ----<|               |  
     |          |          with Access Token     +---------------+  
     |          |            in Fragment  
     |          |                                +---------------+  
     |          |----(D)--- Redirection URI ---->|   Web-Hosted  |  
     |          |          without Fragment      |     Client    |  
     |          |                                |    Resource   |  
     |     (F)  |<---(E)------- Script ---------<|               |  
     |          |                                +---------------+  
     +-|--------+  
       |    |  
      (A)  (G) Access Token  
       |    |  
       ^    v  
     +---------+  
     |         |  
     |  Client |  
     |         |  
     +---------+  
  
   Note: The lines illustrating steps (A) and (B) are broken into two  
   parts as they pass through the user-agent.  
  
                       Figure 4: Implicit Grant Flow  
  
```  
[RFC 6749: The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749#section-4.2)  
  
  
![Pasted image 20221220162619.png](../../attachments/Pasted%20image%2020221220162619.png)  
[Google Oauth2 API Explained. As we all know Google is a powerful… | by Pumudu Ruhunage | Medium](https://medium.com/@pumudu88/google-oauth2-api-explained-dbb84ff97079)  
  
  
**Application** : Application is the entity or which request access to certain google resource owned by different party.  
  
**Resource Owner** : Resource owner is the person/organization whom own the given google resource. This person/organization should allow access to application via login page and consent to access the resources to complete the flow.  
  
**OAuth Service** : Google OAuth service have two endpoints (‘/auth’ and ‘/token’).  
  
“/auth” endpoint provides short lived “authorization code” which confirms user credentials and consent is valid for given scope.  
  
“/token” endpoint exchange code and return bearer and refresh token. With this bearer token and refresh token, application can access the consented resource by resource owner.  
  
Resource Service : Resource service exposes the google resources via structured, well documented set of APIs. Once a request received for this service it will verify the token and execute given request if token is valid.  
  
Resource : This is the actual resource which exposes via APIs. For example, google sheet which creates and maintains by the resource owner is a resource.  
  
  
[Database schema for Django Oauth Toolkit - DrawSQL](https://drawsql.app/templates/django-oauth-toolkit)  
  
![Pasted image 20221220173123.png](../../attachments/Pasted%20image%2020221220173123.png)  
  
  
# 자료  
[Simple OAuth With MongoDB & MySQL - Compose Articles](https://www.compose.com/articles/simple-oauth-with-mongodb-mysql/)  
![Pasted image 20221220173434.png](../../attachments/Pasted%20image%2020221220173434.png)  
  
![Pasted image 20221220173504.png](../../attachments/Pasted%20image%2020221220173504.png)  
  
# 자료  
[javascript - FusionTables private table with OAUTH2 - Stack Overflow](https://stackoverflow.com/questions/10996487/fusiontables-private-table-with-oauth2)  
  
![Pasted image 20221220173947.png](../../attachments/Pasted%20image%2020221220173947.png)  
  
[OAuth | NextAuth.js](https://next-auth.js.org/configuration/providers/oauth)  
여기 플로우 잘 되어있음 (프론트 입장에서)  
![Pasted image 20221230161228.png](../../attachments/Pasted%20image%2020221230161228.png)  
# 자료  
[Grant type sample tables · Issue #5 · bshaffer/oauth2-server-php-docs · GitHub](https://github.com/bshaffer/oauth2-server-php-docs/issues/5)  
![Pasted image 20221220175251.png](../../attachments/Pasted%20image%2020221220175251.png)  
  
  
![Pasted image 20221220175508.png](../../attachments/Pasted%20image%2020221220175508.png)  
  
  
![Pasted image 20221220233036.png](../../attachments/Pasted%20image%2020221220233036.png)  
  
---  
## Oauth2 DB 설계  
[sql - Google Sign-In, Database Side - Stack Overflow](https://stackoverflow.com/questions/18790808/google-sign-in-database-side)  
  
[Postgres 14 | db<>fiddle](https://dbfiddle.uk/Tv6zNb5s)  
  
  
[OAuth 2.0 Refresh Token Best Practices](https://fusebit.io/blog/oauth-refresh-token-best-practices/?utm_source=www.google.com&utm_medium=referral&utm_campaign=none)  
  
  
- Oauth2 response  
```json  
{  
  "access_token": "HEuoFdfRhOwGA0QNn",  
  "refresh_token": "hgTam-tuT8CvFej9-XxGyqeER_7j",  
  "token_type": "bearer",  
  "expires_in": 86400,  
  "refresh_token_expires_in": 525600   
}  
```  
  
365일:  525600  
![Pasted image 20221226150213.png](../../attachments/Pasted%20image%2020221226150213.png)  
---  
  
  
  
# Google Oauth2 + Nest Js Checklist  
- [x] Create Google Oauth2 App in Google Console  
- [x] Create NestJs Project  
- [x] Create Auth Controller  
- [x] Create Auth Service  
- [x] Create Google Strategy  
- [x] Create Google Auth Guard  
- [ ] Connect to a Database  
- [ ] Create a TypeOrm Entity  
- [ ] Configure Sessions  
  
  
> 참고 문서:   
> [Database Setup - OAuth 2.0 Server](https://oauth2.thephpleague.com/database-setup/)  
> [NestJS & Google OAuth2 with Passport - YouTube](https://www.youtube.com/watch?v=OitgkKTxht4&ab_channel=AnsontheDeveloper)  
  
  
로그인 페이지 : Google Login 버튼을 가지고 있다. Login 버튼을 클릭하면 구글 로그인 페이지로 이동한다.  
콜백(callback) 페이지 : 구글 인증이 끝나면, 호출할 페이지. 이 페이지에서 구글 인증 완료를 검증한다.  
  
  
- 구글에 서비스 등록하기  
[Google 클라우드 플랫폼](https://console.cloud.google.com/apis/dashboard)  
  
들어가서  
![Pasted image 20221226150541.png](../../attachments/Pasted%20image%2020221226150541.png)  
  
  
`Oauth consent screen` > external 모드 > create  
  
- 정보 입력 (앱 이름, 내 지메일, 개발자 이메일)  
- `scope` 설정   
  Scopes express the permissions you request users to authorize for your app and allow your project to access specific types of private user data from their Google Account  
- test user 설정  
- `Credentials` > oauth2 client id 생성   
  (Create Credentials > Oauth client ID > Web application 설정)  
    
- Authorized redirect URIs  
  > Users will be redirected to this path after they have authenticated with Google.   
  The path will be appended with the authorization code for access, and must have a protocol. It can’t  
  contain URL fragments, relative paths, or wildcards, and can’t be a public IP address.  
  
http://localhost:3001/api/auth/google/redirect 입력  
  
  
[Implement Google OAuth in NestJS using Passport - DEV Community 👩‍💻👨‍💻](https://dev.to/chukwutosin_/implement-google-oauth-in-nestjs-using-passport-1j3k)  
  
---  
# Nest Js Controller  
global prefix: `api`  
- auth controller  
**Get** api/auth/google/login  
  
**Get** api/auth/google/redirect  
  > Users will be redirected to this path after they have authenticated with Google.   
  The path will be appended with the authorization code for access, and must have a protocol. It can’t  
  contain URL fragments, relative paths, or wildcards, and can’t be a public IP address.  
    
  redirect url to google Oauth screen  
  
```js  
import { Controller, Get } from '@nestjs/common';  
  
@Controller('auth')  
export class AuthController {  
  @Get('google/login')  
  handleLogin() {  
    return { msg: 'Google Authentication' };  
  }  
  
  @Get('google/redirect')  
  handleRedirect() {  
    return { msg: 'OK' };  
  }  
}  
  
```  
  
사용자의 인증이 필요한 경우 Client는 발급받은 JWT를 Requet Header에 실어 같이 보내줍니다.   
Backend는 JWT를 받고 Guard를 통해 JWT Strategy를 실행하고, Secret Key를 통해 JWT를 Decoding합니다.   
JWT를 복호화한 후에 request.user를 통해 User의 정보를 저장하고, User의 정보를 이용해 원하는 API의 Business Logic이 수행된 후 Response 됩니다.  
# Google Strategy  
[passport-google-oauth20 - npm](https://www.npmjs.com/package/passport-google-oauth20)  
```shell  
yarn add @nestjs/passport passport passport-google-oauth20  
yarn add @types/passport-google-oauth20 -D   
```  
[Google OAuth2 Authentication with NestJS explained | by Facundo Lavagnino | Medium](https://medium.com/@flavtech/google-oauth2-authentication-with-nestjs-explained-ab585c53edec)  
  
  
A "<font style="color:indianred">verify callback</font>", which is where you tell Passport how to interact with your user store (where you manage user accounts). Here, you verify whether a user exists (and/or create a new user), and whether their credentials are valid. The Passport library expects this callback to return a full user if the validation succeeds, or a null if it fails (failure is defined as either the user is not found, or, in the case of passport-local, the password does not match).  
  
  
  
We create a GoogleStrategy class that extends PassportStrategy, this class receives 2 parameters, a strategy, and a strategy identifier, we import the strategy from the passport-google-oauth2 package and in the name of the strategy we put ‘google’  
  
In the constructor, we pass a super with the following parameters:  
**clientID**: our client ID provided by google  
**clientSecret**: our client secret provided by google  
**callbackURL**: our redirect URL that we previously configured in the Google console  
**scopes**: an array with the information we want to obtain from the user, the most common is profile and email  
  
In my case I put all the information in environment variables for security reasons, you should do the same.  
  
We then create a function called validate(), which takes 4 required parameters.  
**accessToken**  
**refreshToken**,  
**profile** and a **done** function,  
The access token will be useful to us to interact with Google services, but in our case it is not necessary, we will only use profile, an object with all the user information, and done, a callback function where we will pass the user object and use later to register it in the database and sign the JWT.  
  
You can log the profile object to see what it brings, each provider brings this object in different ways, then you create a user object with the information you need according to your business model and your user schema, and finally you return the user with the done method.  
  
[Nest.js에서 Goolge Oauth 적용하기](https://velog.io/@sinf/Nest.js에서-Goolge-Oauth-적용하기)  
  
  
리다이렉션 Controller를 간단하게 요청 URL에 맞춰 UseGuards 에 AuthGuard(’google’) 을 전달한다. AuthGuard에 google을 전달하면, Strategy를 작성할 때 작성한 명칭을 찾아 적용한다.  
  
---  
  
# AuthService  
유저가 존재하는지 확인하기 위해서 db 연결 , 에러핸들링,  
  
  
  
# 직렬화  
[NestJS에서 응답/요청 객체 직렬화 (Serialization) 하기](https://jojoldu.tistory.com/610)  
오오.. 인터셉터 써서 json으로 직렬화한다함...  
  
# Authorization  
[OAuth2 in NestJS for Social Login (Google, Facebook, Twitter, etc) | by Csaba Apagyi | JavaScript in Plain English](https://javascript.plainenglish.io/oauth2-in-nestjs-for-social-login-google-facebook-twitter-etc-8b405d570fd2)  
  
  
  
[OAuth with NestJS application – sign in with Google | Just a blog](https://blog.dominikwawrzynczak.pl/2020/08/oauth-with-nestjs-application-sign-in-with-google/)  
  
  
# Flow  
[NestJS 회원가입 및 로그인(DB저장)](https://velog.io/@ehgks0000/NestJS-DB저장하기TypeORM)  
  
**구글 로그인 Flow**  
유저 구글 로그인 -> localhost:3000/auth/google 접속 -> 성공 시 localhost:3000/auth/google/callback 접속 -> req.user를 JSON으로 반환(나중에 JWT 발급 추가)  
  
  
**JWT Flow**  
유저 local 로그인 -> authService.login 실행 -> user와 jwt Token반환 -> localhost:3000/auth/profile 접속(Token이 유효한지 확인) -> req.user 반환  
  
  
# Done : verifyCallback이 뭘까  
[validate function is called with the wrong number of arguments · Issue #446 · nestjs/passport · GitHub](https://github.com/nestjs/passport/issues/446)  
  
  
[원문](https://dev-insights.hashnode.dev/implement-google-oauth-in-nestjs-using-passport) In this method, you decide what to do with the user information returned by Google. You then return the result using the `done` method.  
[원문](https://github.com/mikenicholson/passport-jwt#extracting-the-jwt-from-the-request) done is a passport error first callback accepting arguments done(error, user, info)  
  
[원문](https://www.zerocho.com/category/NodeJS/post/57b7101ecfbef617003bf457) 비교 과정에서 서버 에러가 나면 done(findError)로 에러를 리턴하고, 비밀번호가 맞을 경우 done(null, user);로 user 객체를 전송해주고, 틀렸을 경우는 비밀번호가 틀렸다고 done(null, false, { message: '에러메시지' })로 메시지를 전송합니다.  
  
done이 인자를 세 개나 받아 헷갈릴 수도 있는데 다음과 같습니다. 첫 번째 인자는 DB조회 같은 때 발생하는 서버 에러를 넣는 곳입니다. 무조건 실패하는 경우에만 사용합니다. 두 번째 인자는 성공했을 때 return할 값을 넣는 곳이고요. 성공했으면 당연히 첫 번째 인자는 null이어야겠죠? 에러가 있으면 안 되니까요. 세 번째 인자는 언제 사용하나면, 사용자가 임의로 실패를 만들고 싶을 때 사용합니다. 첫 번째 인자를 사용하는 경우는 서버에서 에러가 났을 때 무조건 실패하는 경우라고 했죠. 세 번째 인자는 위에서 비밀번호가 틀렸다는 에러를 표현하고 싶을 때 사용하면 됩니다. 이것은 서버 에러도 아니고, 사용자가 임의로 만드는 에러이기 때문에, 직접 에러 메시지도 써주는 겁니다.  
  
  
[passport-google-oauth20](http://www.passportjs.org/packages/passport-google-oauth20/) 설명에 나와있는 라이브러리 내부 코드를 뜯어보자  
[passport-google-oauth2/strategy.js at master · jaredhanson/passport-google-oauth2 · GitHub](https://github.com/jaredhanson/passport-google-oauth2/blob/master/lib/strategy.js)  
를 보면 cb 함수가 done이다.   
  
  
[javascript - What is "done" callback function in Passport Strategy Configure "use" function - Stack Overflow](https://stackoverflow.com/questions/32153865/what-is-done-callback-function-in-passport-strategy-configure-use-function) 에 글에 따르면  
  
The passport documentation has improved and it describes the done method (also called cb) here:  
  
  
[passport-google-oauth2/strategy.js at master · jaredhanson/passport-google-oauth2 · GitHub](https://github.com/jaredhanson/passport-google-oauth2/blob/master/lib/strategy.js#L84)  
  
여기 라인 보면 `function (err, body, res)` 이게 done 함수 구현체다.  
# Oauth with fetch() in Vanila JS  
  
  
  
[출처](https://wanago.io/2021/07/26/api-nestjs-google-authentication/)  
The last step in authenticating our users is receiving the accessToken from Google and logging the user into our system.  
[Site Unreachable](https://developers.google.com/identity/protocols/oauth2/javascript-implicit-flow)  
  
  
[API with NestJS #3. Authenticating users with bcrypt, Passport, JWT, and cookies](https://wanago.io/2020/05/25/api-nestjs-authenticating-users-bcrypt-passport-jwt-cookies/)  
  
  
  
  
google-oauth.guard.ts  
```ts  
import { Injectable } from '@nestjs/common';  
import { ConfigService } from '@nestjs/config';  
import { AuthGuard } from '@nestjs/passport';  
  
@Injectable()  
export class GoogleOauthGaurd extends AuthGuard('google') {  
  constructor(private configService: ConfigService) {  
    super({  
      accessType: 'offline',  
    });  
  }  
}  
  
```  
  
![Pasted image 20230101145713.png ](Pasted%20image%2020230101145713.png)  
[출처](https://sddev.tistory.com/217)  
FE 가 BE로 요청  
사용자는 로그인하고 Google은 사용자 정보와 함께 BE를 호출  
BE가 사용자 정보를 처리(회원가입/로그인 + JWT 토큰 생성)  
BE가 JWT와 함께 FE로 사용자를 redirect시킴  
FE는 JWT를 등록 후, BE API 호출 시, 요청 header에 Token을 사용함  
  
  
  
[NestJS : JWT 토큰/ Config / Pagination — devNote](https://sddev.tistory.com/94?category=873276)  
  
  
[Creating social logins in NestJS - LogRocket Blog](https://blog.logrocket.com/social-logins-nestjs/)  
  
  
# Refresh Token  
[12) 개인 프로젝트) NestJS Auth Refresh Token](https://velog.io/@algo2000/pj01-12)  
  
`TODO`: user id를 uuid로 받아야겠음.  
[Notion – The all-in-one workspace for your notes, tasks, wikis, and databases.](https://doritos2.notion.site/Refresh-Token-63a0d691cba94245a2b43f45d3cafdba)  
  
[Mobile Convergence :: [MS-7] Refresh Token 설정](https://mobicon.tistory.com/593)  
Auth Token (Forbidden)오류 발생시 RefreshToken을 통해 Auth Token 재생성한다.   
  
# @Res(): Library-specific Approach  
```ts  
import { Body, Controller, Get, HttpStatus, Post, Res } from '@nestjs/common';  
import { CreatePostDto } from './create-post.dto';  
import { Response } from 'express';  
  
@Controller('post')  
export class PostController {  
  // library specific approach  
  @Get()  
  findAll(@Res() res: Response) {  
    res.status(HttpStatus.CREATED).send();  
  }  
  
  @Post()  
  create(@Body() createPostDto: CreatePostDto, @Res() res: Response) {  
    res.status(HttpStatus.OK).json(createPostDto);  
  }  
}  
```  
  
[원문](https://velog.io/@jujube0/NestJS로-REST-API-만들기) Library-specific approach 단점?  
코드가 플랫폼에 의존성을 띄게 됨  
테스트가 어려워짐(response object를 만들어줘야함)  
Nest standard response handling(@HttpCode(), @Header(), interceptors)을 이용할 수 없음 → 이를 고치기 위해서는 @Res({ passthrough: true}) 를 추가해준다.  
  
  
[컨트롤러 2편 - 쉽게 풀어 쓴 Nest.js](https://www.wisewiredbooks.com/nestjs/overview/03-controller-2.html)  
```ts  
import { Controller, Get, Post, Res, HttpStatus } from '@nestjs/common';  
import { Response } from 'express';  
  
@Controller('cats')  
export class CatsController {  
  @Post()  
  create(@Res() res: Response) {  
    res.status(HttpStatus.CREATED).send();  
  }  
  
  @Get()  
  findAll(@Res() res: Response) {  
    res.status(HttpStatus.OK).json([]);  
  }  
}  
  
```  
이 방식은 그래도 잘 작동하긴 하지만, 응답 객체(헤더 조작, 라이브러리 별 기능 등)에 대한 모든 제어권을 다루기 때문에 더 많은 유연성을 허용합니다. 권한이 크면 책임도 큰 법이기 때문에 주의해서 사용해야 합니다. 일반적으로 이 방식은 훨씬 덜 명확하고 몇 가지 단점이 있습니다. 가장 큰 단점은 코드가 플랫폼에 종속된다는 점입니다. Express 라면 Express, Fastify라면 Fastify 전용 코드를 사용하기 때문입니다. 나중에 Express에서 Fastify 혹은 그 반대로 하위 계층(웹 서버)을 변경한다면 코드를 다 뜯어 고쳐야 합니다. 또한 두 번째는 테스트하기가 어렵습니다. 응답 객체를 Mocking해야 하기 떼문입니다.  
  
또한 위의 예제에서 인터셉터 및 @HttpCode()/ @Header()데코레이터와 같은 Nest 표준 응답 처리에 의존하는 Nest 기능과의 호환성이 사라집니다. 이 문제를 해결하려면 다음과 같이 passthrough 옵션을 true로 설정하시기 바랍니다.  
  
  
![Pasted image 20230101220048.png](../../attachments/Pasted%20image%2020230101220048.png)  
[사진 출처](https://medium.com/a-layman/jwt-authentication-in-nestjs-refresh-jwt-with-cookie-based-token-2f6b860f7d67)  
  
  
Frontend calls google/login -> GoogleAuthGaurd -> GoogleStrategy -> google/redirect -> Generate custom JWT token -> redirect to frontend with access token and refresh token in URL.  
  
  
# Access Token 오류 (ms 착각함..)  
토큰 유효 숫자를 600ms 로 해뒀다.............................................................  
이런 실수 많이 발생해서 util에 따로 익명 함수 간단한걸로 명시적으로 적어둠  
- src/util/time-transform.util.ts  
```Ts  
export const secondToMilisecond = (seconds: number) => seconds * 1000;  
export const dayToMilisecond = (days: number) => days * 3600 * 24 * 1000;  
```  
  
generate random password  
  
  
wip (work in progress): [Work-In-Progress (WIP) commits: a git technique in Trunk-Based Development](https://www.dmitriydubson.com/post/trunk-dev-wip-commits/) 이후 `rebase`   
  
디버깅하는 법  
→ 값을 잘못 줬나? 생각하기   
→ 값이 올바르다는 걸 확인한 다음에 구조를 확인하자  
  
# console.log 대신 Logger로 찍자  
```ts  
private logger: Logger = new Logger(AuthController.name);  
this.logger.log('token: ', token);  
```  
  
  
  
→ 프론트에서 쿠키 넣어줘야하는 것 아님?  
Next.js 에서 모든 요청마다 cookie 에다 넣어서 요청하는거 해보기  
  
# Cors  
[Can't get cookies from request in NestJS - Stack Overflow](https://stackoverflow.com/questions/68807522/cant-get-cookies-from-request-in-nestjs)  
```ts  
const app = await NestFactory.create(AppModule, { cors: {credentials: true, origin: process.env.CLIENT_URL} })  
```  
  
# Role-Based Authorization  
[Role-Based Authorization with JWT Using NestJS](https://shpota.com/2022/07/16/role-based-authorization-with-jwt-using-nestjs.html)  
  
  
  
# Header 읽기  
api 요청으로 하는게 아님  
프론트 단에서 붙여서 넣어주는거임  
  
[Set-Cookies Header and withCredentials | by Adam Drake | Medium](https://adam-drake-spiritual-seeker.medium.com/set-cookies-header-and-withcredentials-c5d15c9914b9  
  
[여기](https://security.stackexchange.com/questions/245539/is-it-possible-to-read-and-extract-http-request-headers-via-javascript-while-per) 는 뭐지  
  
아무튼... 쿠키로 읽으면 ㄱㅊ음  
  
  
  
# Express Cookie  
  
[Node Express를 이용한 Cookie Set](https://rpdmfms.tistory.com/11)  
[Express 4.x - API Reference](https://expressjs.com/en/4x/api.html)  
![Pasted image 20230103200912.png](../../attachments/Pasted%20image%2020230103200912.png)  
  
  
[javascript - How to store, read and delete cookies and sessions in Nest.js - Stack Overflow](https://stackoverflow.com/questions/55947948/how-to-store-read-and-delete-cookies-and-sessions-in-nest-js)  
  
jdbc:redis://localhost:6381/0  
  
# Refresh Strategy 파일 에러..  
## 쿠키 값을 못읽음.  
[GitHub - mikenicholson/passport-jwt: Passport authentication using JSON Web Tokens](https://github.com/mikenicholson/passport-jwt#configure-strategy)  
  
여기서 보니까 extractor function을 따로 구현해야지 값을 읽어와줄 수 있는 것 같다.  
`Writing a custom extractor function` 항목이 있음  
  
`verify` is a function with the parameters `verify(jwt_payload, done)`  
  
- `jwt_payload` is an object literal containing the decoded JWT payload.  
- `done` is a passport error first callback accepting arguments done(error, user, info)  
  
## ModuleRef 가 뭘까  
[Nest.js - moduleRef를 활용 해보자](https://malgogi-developer.tistory.com/37)  
아무리해도 refresh 토큰 stratge 파일에서   
component를 type에 따라서 다르게 처리해야하는 경우에 사용함.  
자바 spring 에서 beanFactory 와 유사하다.  
  
  
## Request Scope 가 뭐지  
[[Spring]Request Scope를 사용해서 깔끔하게 로그남기기](https://chung-develop.tistory.com/64)  
[웹 프로그래밍(풀스택) > 3) request scope : 부스트코스](https://www.boostcourse.org/web316/lecture/16710?isDesc=false)  
request scope를 활용하면 아래와 같이 디버깅하기 쉬운 로그환경을 만들 수 있다.  
  
- http 요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지 변수값을 유지하고자 할 경우 사용한다.  
  
[A Deep Dive Into the NestJS Injection Scope - DEV Community 👩‍💻👨‍💻](https://dev.to/this-is-learning/a-deep-dive-into-the-nestjs-injection-scope-39ih)  
[Injection scopes | NestJS - A progressive Node.js framework](https://docs.nestjs.com/fundamentals/injection-scopes)  
  
A new instance of the provider is created exclusively for each incoming request. The instance is garbage-collected after the request has completed processing.  
  
  
대충 그때그때 띄워주는 간이 프로바이더군아..  
  
## passReqToCallback 왜 못받지  
[(NodeJS) Passport로 회원가입 및 로그인하기 - ZeroCho Blog](https://www.zerocho.com/category/NodeJS/post/57b7101ecfbef617003bf457)   
보니까 콜백 인자를 req 로 받는다고 함..  
걍 request 해도 받아지는뎅;  
  
  
# Does Every Web Request Send the Browser Cookies?  
[Does every web request send the browser cookies? - Stack Overflow](https://stackoverflow.com/questions/1336126/does-every-web-request-send-the-browser-cookies)  
Yes, as long as the URL requested is within the same domain and path defined in the cookie (and all of the other restrictions -- secure, httponly, not expired, etc) hold, then the cookie will be sent for every request.  
  
## Cookie Options 확인  
[브라우저 쿠키와 SameSite 속성 / seob.dev](https://seob.dev/posts/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EC%BF%A0%ED%82%A4%EC%99%80-SameSite-%EC%86%8D%EC%84%B1/)  
  
`SameSite`: 쿠키를의 SameSite 정책을 의미합니다. 해당 설정에 따라 도메인 당 쿠키 설정이 제한됩니다.  
`Domain`: 쿠키를 전송할 도메인을 의미합니다. 해당 도메인에서만 쿠키가 유효해집니다.  
`Path`: 쿠키를 전송할 Path를 의미합니다. Path가 / 가 아닌 경우 지정된 페이지에서만 쿠키가 전송됩니다.  
`Expires`: 쿠키의 유효기간입니다. 파일 쿠키에 사용됩니다. (세션쿠키는 브라우저 탭 종료 시 만료됩니다)  
`Secure`: true로 설정되는 경우 TLS(https) 환경에서만 쿠키가 전송됩니다.  
`HttpOnly`: true로 설정되는 경우 Javascript에서 document.cookie로 접근할 수 없습니다.  
  
  
### XSS Attack (Cross Site Scripting): HttpOnly  
[Protecting Your Cookies from Cross Site Scripting (XSS) Vulnerabilities – How XSS Works](https://www.firewall.cx/general-topics-reviews/web-application-vulnerability-scanners/1199-protecting-user-cookies-from-xss-vulnerabilities-attacks.html)  
[How HttpOnly cookies help mitigate XSS attacks](https://clerk.dev/blog/how-httponly-cookies-help-mitigate-xss-attacks)  
  
  
![Pasted image 20230109160730.png](../../attachments/Pasted%20image%2020230109160730.png)  
해커가 그대로 베낀 웹사이트를 유저에게 보여주고 입력하게 시킴.  
유저는 순진하게 자기 쿠키 다 줌  
해커가 쿠키 정보 디코드 해서 진짜 사이트에 유저인 척 행세함.  
해커가 유해한 스크립트 파일을 웹 서버에 post함  
  
A cookie with the HttpOnly attribute is inaccessible to the JavaScript <font style="color:purple">Document.cookie</font> API;  
  
브라우저에서 쿠키에 접근할 수 없도록 제한함.  
  
  
### Secure Cookies  
HTTPS 아닌 통신에서는 쿠키를 전송하지 않는다.  
  
Setting secure: true in development environment will prevent the server from sending the cookies to the browser or client.  
  
### CSRF (Cross-Site Request Forgery): Same Site  
[CSRF (Cross-Site Request Forgery)](https://www.hahwul.com/cullinan/csrf/)  
참고: 20년 2월부터 크롬에서는 SameSite = Lax 가 기본값으로 변경되었다.  
  
SameSite 옵션에 대한 자세한 설명 글은 [여기](https://www.hahwul.com/2020/01/18/samesite-lax/) 에서 읽어보자.  
쿠키의 동작 방식을 이용한 공격 방법으로 사용자의 세션 쿠키 또는 다른 인증정보를 이용하여 사용자 모르게 공격자가 의도한 서비스 요청을 처리하는 공격 방법을 의미합니다.  
  
CSRF의 공격 메커니즘은 단순합니다. 서비스의 기능 요청을 크로스 도메인 환경, 즉 공격자가 구성한 웹 페이지를 이용하여 사용자의 인증정보를 포함해 전송하기만 하면 됩니다.   
일반적으로 GET CSRF의 경우 img 태그, POST 등 다른 메소드를 이용한 CSRF는 form 태그가 많이 활용됩니다.  
  
  
**Authorization Header CSRF**  
일반적으로 HTTP Header를 통한 인증은 Javascript 단에서 이루어지며 CSRF를 성공하기 어려운 조건 중 하나가 됩니다. 다만 예외적으로 Authorization 헤더의 경우 HTML Form으로 요청 후 받아오는 경우 브라우저에서 자동으로 Authorization 헤더를 붙이기 때문에 이를 통해 인증헤더가 포함된 CSRF를 수행할 수 있습니다.  
  
- **🛡 Defensive techniques**  
방어 매커니즘도 공격 매커니즘과 동일하게 간단합니다. 이 기능을 수행하는 요청이 크로스도메인, 또는 내가 신뢰하는 도메인에서 온 요청인지만 정확하게 검증하면 됩니다. 이를 위한 기술로 대표적인 것은 CSRF Token, Referer 헤더 검증, Origin 헤더 검증이 있으며 최근에 브라우저의 쿠키 정책의 변화로 인해서 잘 설정된 쿠키 설정으로도 충분히 막을 수 있습니다.  
  
**CSRF Token**  
CSRF Token은 보통 중요한 기능에 대해 CSRF를 대응하기 위해 많이 사용되는 방법이며, 기능을 수행하기 위한 페이지에서 추측할 수 없는 길이의 임의 토큰을 발급하고, 해당 토큰을 포함한 요청만 처리하도록 하여 해당 기능이 실행되어야 하는 곳에서만 처리될 수 있도록 제한하는 기술입니다.  
  
다만 이 과정에선 정확하게 Token을 검증해야하며, Token에 대한 보안성도 충분히 고려되야 합니다.  
  
쉽게 크랙할 수 없는 긴 길이의 토큰을 사용합니다.  
토큰의 값은 추측하거나 역분석이 어려워야 합니다.  
토큰은 가급적 매번 기능 페이지 요청 시 새로 발급되어야 하며, 사용된 토큰은 폐기처리 해야합니다.  
토큰은 각 유저 세션당 유니크 해야합니다.  
CSRF 토큰은 HTTP Header 또는 URI, Body 등으로 전달되어야 합니다. (쿠키는 위험해요)  
  
  
**Token + Cookie**  
CSRF Token의 보안성을 강화하기 위한 방법으로 CSRF Token을 header 또는 body 값을 통해 전달하는 방법 이외에 HttpOnly Cookie로도 전달하여 이중으로 검증하는 방법이 있습니다. 이렇게 되는 경우 어떤 액션을 통해 CSRF Token이 유추되거나 탈취되어도 보안성이 요구된 Cookie를 통해 동시에 전달할 수 없기 때문에 2차적인 피해를 예방할 수 있습니다.  
  
## 해결: jwtFromRequest 함수 문제였음  
라이브러리 깃허브 문서 찾아서 커스텀 함수 붙임   
  
사실 커스텀 함수 젤 첨에 붙엿엿는데 안되다가 서버 껏다 키니까 갑자기 됨;; 머지;  
  
  
# 추가 참고하면 좋을 문서  
[Spotify OAuth2 Authentication in a NestJS Application | by Marcus | Better Programming](https://betterprogramming.pub/spotify-oauth2-authentication-in-a-nestjs-application-307b25b2e49e)  
  
여기도 좋은듯.. (이미 다 구현한 뒤에 봤지만..)  
  
# Test Authorization  
더 자세히: [Test 관련 의식 흐름](../Testing/Test%20%EA%B4%80%EB%A0%A8%20%EC%9D%98%EC%8B%9D%20%ED%9D%90%EB%A6%84.md) 에 정리함  
  
[Authentification with NestJs and clean architecture | by Jonathan Pretre | Medium](https://medium.com/@jonathan.pretre91/authentification-with-nestjs-and-clean-architecture-182e44a7ed07) 음 무슨 계층 구조 설명은 유용한데 그 외는 그닥..  
  
[Unit Test Token Verification for Auth0 using Jest and mock-jwks | by Jason Brown | codeburst](https://codeburst.io/unit-test-token-verification-for-auth0-using-jest-and-mock-jwks-2c8488df97d6)  
봐도 모르겟다....  
  
  
[API with NestJS #9. Testing services and controllers with integration tests](https://wanago.io/2020/07/13/api-nestjs-testing-services-controllers-integration-tests/)  
진짜 이거 보고 해보겟음!!  
  
  
  
req 객체  
```ts  
req: {  
  cookies: {  
    ...  
    Authentication: "...",  
    Refresh: "...",  
  }  
   
  ...  
  rawHeaders: {  
    ...  
    "Authentication=...; Refresh=..."  
  }  
}  
```  
  
---  
# 리팩토링 해야할 것  
리프레시 토큰 어떻게 저장하지  
  
[https://github.dev/jengel3/nestjs-auth-example/tree/master/src/modules/authentication](https://github.dev/jengel3/nestjs-auth-example/tree/master/src/modules/authentication)   
  
  
[API with NestJS #13. Implementing refresh tokens using JWT](https://wanago.io/2020/09/21/api-nestjs-refresh-tokens-jwt/)  
참고하면서 코드 리팩토링  
  
  
[@nestjs/jwt](https://www.npmjs.com/package/@nestjs/jwt) 문서  
  
[What Are Refresh Tokens and How to Use Them Securely](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)  
  
  
[Refresh Token in Web API with Examples - Dot Net Tutorials](https://dotnettutorials.net/lesson/refresh-token-in-web-api/)  
Bounding the refresh token to a client is very important this is because you do not want any refresh token generated by your Authorization Server to be used by another client to obtain the access token.  
  
  
## Refresh 토큰 저장 보안 문제  
[authentication - How to handle refresh tokens - Information Security Stack Exchange](https://security.stackexchange.com/questions/194774/how-to-handle-refresh-tokens)  
  
It's better to store a refresh token locally. If the database would get hacked, the hacker would have unlimited access to the accounts using the refresh tokens.  
  
  
workflow would be:  
  
1. User logs in, gets access and refresh token. Access token lifetime 15min, refresh token 5 days.  
  
2. User accesses the service using the access token. Service only checks signature and lifetime. No database connection.  
  
3. User logs out, the refresh token is marked expired in the **database**  
4. User accesses the service using the access token, this still works  
5. 15min pass. Access token expires, user requests a new access token using the refresh token still within its lifetime. The service checks the **database** and finds the token is expired. User can't get a new access token.  
  
You can replace the refresh token on each refresh, but remember that you need to **store all expired refresh tokens until their lifetime is over.**  
  
  
  
[Part-2 A Demo On JWT Access Token And Refresh Token Authentication In .NET6 Web API](https://www.learmoreseekmore.com/2022/06/part2-demo-on-jwt-access-token-and-refresh-token-authentication-in-dotnet6-webapi.html) 를 보니까 refresh 토큰 따로 저장함  
  
- refresh-token.entity.ts  
```ts  
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';  
  
@Entity('user.refresh_token')  
export class RefreshToken {  
  // TODO: user 1:1 관계 설정하기  
  @PrimaryGeneratedColumn()  
  id: number;  
  
  @Column()  
  hashedRefreshToken: string;  
  
  @Column()  
  userId: string;  
  
  @Column()  
  isRevoked: boolean;  
  
  @Column()  
  expires: Date;  
}  
  
```  
  
일단 패스...  
  
  
저장 위치만 생각해보자.  
  
  
[JWT 혹은 OAuth2 의 refresh 토큰을 어디다 저장해야 할까? | 두글 블로그](https://doogle.link/jwt-%ED%98%B9%EC%9D%80-oauth2-%EC%9D%98-refresh-%ED%86%A0%ED%81%B0%EC%9D%84-%EC%96%B4%EB%94%94%EB%8B%A4-%EC%A0%80%EC%9E%A5%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C/)  
1. Local Storage  
처음에는 local storage 나 쿠키(Cookie)에 저장하는 방법을 생각해 보았는데요. local storage 는 자바스크립트로 접근이 너무 쉬워서 XSS 공격에 취약하고 보안상 문제 소지가 많아 보입니다.  
  
2. 쿠키 (Cookie)  
쿠키의 경우는 HTTPOnly 와 Secure 옵션을 사용하고 CSRF 공격에 대비를 하면 어느정도 보안이 되기때문에 한때는 여기다가 리프레시 토큰을 저장하곤 했습니다.  
  
현재 JWT 형식의 토큰의 경우는 토큰 자체가 추가적인 값을 가지기도 하고 암호화되기 때문에 HTTPOnly 토큰이 가장 효율적인 것으로 생각됩니다. 다만 HTTPOnly 쿠키는 클라이언트에서 토큰 접근이 안되므로 토큰에 들어있는 정보를 접근하려면 관련 처리를 서버 쪽에서 해줘야 합니다.  
  
하지만 쿠키도 역시 불안하기는 마찬가지라서 좀 더 좋은 방법이 없나 궁리를 해보게 되는데요.  
  
1. 결국은 Server Side  
역시 가장 좋은 방법은 서버사이드에 저장해두는 것이겠죠.  
  
가장 간단한 방법은 세션에 저장하고 세션 만료 주기를 왕창 늘려주는 방법도 있는데요. 이건 사용자가 얼마 안 된다면 써먹어 볼 수 있겠지만 역시 사용자가 많은 경우에는 말도 안 되는 방식이고… 애초에 세션을 사용하지 않으려고 만든 게 액세스 토큰 방식인데 뭔가 아닌거 같더라고요.  
  
그래서 생각해 낸 것이 DB에 저장하는 방법입니다.  
  
DB에 실제 리프레시 토큰 값을 저장하고 이때 index 값을 쿠키나 로컬 스토리지에 저장하는 방법입니다. 쿠키에 저장하는 경우 만료 기간을 한 달이나 1년 등으로 길게 잡으면 충분히 끊임없이 로그인 된 상태를 유지할 수 있습니다.  
  
이렇게 하면 리프레시 토큰 값은 노출시키지 않고 크게 의미 없는 index 값 만 노출되므로 보안상 좀 더 안전하게 저장할 수 있습니다.  
  
이를 조금 응용해서 저장하는 인덱스 값도 유추가 쉬운 단순 순번 값보다는 사용자 아이디와 추가적인 값을 조합해 SHA256 등을 활용해 hash값을 생성해 사용하면 보안에 더욱 유리할 것입니다. 물론 DB에도 리프레시 토큰 값만이 아니라 사용자 아이디 값을 추가로 저장해야 합니다.  
  
이렇게 되면 결과적으로 리프레시 토큰 값에 대한 해시테이블(hash table) 혹은 해시 맵(hash map)을 구현하게 됩니다.  
  
마지막으로 이 해시값을 HTTP Only, Secure 옵션을 걸어놓은 쿠키로 저장해서 사용하면 됩니다. 즉 2번 쿠키 저장방법과 혼합해서 사용하면 됩니다.  
  
참고로 카카오 개발자 페이지에서 카카오 로그인 Javascript 문서 중 refresh token 관련된 정보(https://devtalk.kakao.com/t/javascript-api-sdk-refresh-token/105942)를 봐도 클라이언트 단에서 refresh token 을 노출하지 않도록 REST API로만 처리할 수 있도록 정책이 바뀌고 있는 것을 확인할 수 있습니다.  
  
https://developers.kakao.com/docs/latest/ko/kakaologin/js#advanced-guide  
  
만료기간이 짧은 access token 의 경우에는 저장 위치를 클아이언트단에서도 할 수 있지만 아무래도 만료기간이 긴 refresh token 의 경우 탈취된다면 크게 문제가 될 수 있기 때문이라고 생각됩니다. refresh token 은 가능한한 쉽게 접근할 수 없는 곳에 저장하는 것이 정답인듯 합니다. ^^  
  
  
  
  
---  
  
# 관련 문서  
[로그인 리팩토링 - Redis 사용한 토큰 관리](./%EB%A1%9C%EA%B7%B8%EC%9D%B8%20%EB%A6%AC%ED%8C%A9%ED%86%A0%EB%A7%81%20-%20Redis%20%EC%82%AC%EC%9A%A9%ED%95%9C%20%ED%86%A0%ED%81%B0%20%EA%B4%80%EB%A6%AC.md)  
  
  
[4장. JWT 이해 및 적용 · GitBook](https://backend-intro.vlpt.us/4/)  
  
서버 기반 인증의 문제점  
서버기반 인증의 문제점은 크게 2가지로 있습니다.  
  
첫째는, 세션을 유지하게 될 때, 로그인중인 유저의 수가 많아진다면 성능에 무리가 가게 됩니다. 이 정보를 메모리에 넣게 된다면 램이 과부화가 되고, 데이터베이스에 넣게 된다면 데이터베이스의 성능에 무리가 가게 됩니다.  
  
두번째는, 서버 확장이 어려워진다는 점입니다. 여기서 서버 확장은, 단순히 서버의 사양을 업그레이드 하는 것 말고, 더 많은 트래픽을 감당하기 위하여 여러 프로세스를 돌리거나, 여러 서버 컴퓨터를 추가하여 로드밸런싱을 할때를 의미합니다. 세션을 사용하면서 분산된 시스템을 설계하는건, 세션의 정보가 분산된 프로세스간에 동기화가 되야하므로, 과정이 매우 복잡합니다.  
  
토큰 기반 시스템이 제공해주는것은?  
토큰 기반 시스템은 stateless 합니다. 이 용어의 의미는 '무상태' 라는 뜻 인데요. 서버시스템측에서 더 이상 유저의 정보를 유지하지 않고, 유저가 회원 인증을 하게 될 때 토큰 을 발급해줌으로서 유저가 자기 자신임을 인증 할 수 있게 해줍니다. 발급이 된 토큰은, 토큰의 유효기간, 그리고 정보를 담고 있으며, 해싱 알고리즘을 통해 인증이 되어있어서 서버에서 검증을 통하여 처음 서버가 발급해주었던 정보가 변조되지 않았음을 보장 해 줄 수 있습니다.  
  
토큰을 사용함으로서, 서버를 확장하게 될 때에 매우 용이해지게 됩니다. 서버 시스템이 분산이 되어있어도, 유저는 같은 토큰으로 서버에 요청을 하면 되고, 서버는 그저 그 토큰이 위조되지 않았는지만 검증을 하고 데이터베이스 조회도 할 필요 없이 바로 유저임을 신뢰하고 처리를 하면 되기 때문이죠.  
  
추가적으로, 토큰을 사용하면 플랫폼간 권한을 공유 할 수 있습니다. 예를들어서, 우리가 다음 강의에서 페이스북 / 구글 계정을 통한 소셜 로그인을 구현하게 될 텐데, 이게 가능한 이유도 구글과 페이스북에서 토큰기반인증 시스템을 사용하기 때문입니다. 소셜 로그인 과정에서, 구글/페이스북 플랫폼에서 로그인을 하고, 해당 플랫폼이 토큰을 발급을 해주면 우리의 백엔드 서버에서 이를 통하여 회원정보를 가져오고 우리의 서비스에 계정 생성을 하게 됩니다.  
  
마지막으로, 토큰 기반 시스템은 모바일 어플리케이션에서 사용하기에 편해집니다. 만약에 세션 기반 인증을 사용한다면, 쿠키를 사용해야 하기 때문에, 쿠키 매니저를 따로 관리해줘야 하지만, 토큰을 사용한다면 웹 요청 API 에 헤더에 넣어서 사용해주면 되기때문에 더이상 쿠키 매니저를 사용 할 필요가 없어지죠.  
  
  
토큰 저장 위치  
서버가 토큰을 발급해주면, 브라우저에서 사용자 / 서버간에 토큰이 전달되는 방식은 크게 두가지로 나뉩니다. 첫번째는, 로그인에 성공하게 되었을 때 서버가 토큰을 응답정보에 토큰을 넣어서 전달하도록 하고, 해당 값을 웹 스토리지 (localStorage 혹은 sessionStorage) 에 넣고, 그 다음부터 웹 요청을 할 때마다 HTTP 헤더 값에 넣어서 요청하는 방법이 있습니다.  
  
이 방법은, 구현하기 쉽고, 하나의 도메인에 제한되어있지 않다는 장점이 있지만, XSS 해킹 공격을 통하여 해커의 악성스크립트에 노출이 되는 경우 매우 쉽게 토큰이 탈취 될 수 있습니다. 그냥 localStorage 에 접근하면 바로 토큰에 접근 할 수 있기 때문이죠.  
  
이에 대한 대안, 두번째 방식은 이 토큰을 쿠키에 넣는 것 입니다. 쿠키를 쓰지 않으려고 토큰을 쓰는 줄 알았는데, 갑자기 이렇게 설명을 하니 헷갈릴 수도 있습니다. 쿠키를 사용한다고해서 세션을 관리하는것은 아니고, 그저 쿠키를 정보 전송수단으로 사용 할 뿐입니다. 이 과정에서, 서버측에서 응답을 하면서 쿠키를 설정 해 줄 때 httpOnly 값을 활성화를 해주면, 네트워크 통신 상에서만 해당 쿠키가 붙게 됩니다. 따라서, 브라우저상에서는, 자바스크립트로 토큰값에 접근하는것이 불가능해지죠.  
  
만약에 모바일 어플리케이션도 개발을 하게 된다면, 서버측에서 JWT 를 헤더 값으로도 받을 수 있게 하여 쿠키 혹은 헤더 둘 중 존재하는것을 토큰으로 인식하여 사용하도록 구현을 하면 됩니다.  
  
두번째 대안에 대한 단점은, 쿠키가 한정된 도메인에서만 사용이 된다는 점 입니다. 이 부분은, 토큰이 필요해질 때 현재 쿠키에 있는 토큰을 사용하여 새 토큰을 문자열로 받아올 수 있게 하는 API를 구현하여 해결하면 됩니다.  
  
또 다른 단점은, XSS의 위험에서 완벽히 해방되는 대신, CSRF 공격의 위험성이 생긴다는 점 입니다. CSRF은, 계정정보를 탈취하는것은 아니지만, 스크립트를 통하여 사이트의 외부에서 사이트의 API 를 사용하는것 처럼 모방하는 것 입니다. 혹은, 사이트 내부에서 스크립트가 실행되어 원하지 않는 작업이 수행되게 할 수도 있습니다.  
  
CSRF 공격의 예제는 다음과 같은 항목들이 있습니다.  
  
스크립트에 노출되는 순간...  
  
유저도 모르는사이 탈퇴해버리기  
덧글을 자동으로 작성  
포스트를 자동으로 작성  
회원정보 변경  
하지만, 너무 걱정하지는 마세요, CSRF의 경우엔 HTTP 요청 레퍼러 체크, 그리고 CSRF 토큰의 사용을 통하여 방지 할 수 있습니다.  
  
정리를 하자면, 웹스토리지에 토큰을 담으면 XSS 에 취약하고, 쿠키에 담으면 CSRF 에 취약해집니다. 하지만, CSRF는 보안에 신경을 쓰면 차단 할 수 있습니다.  
  
우리는 앞으로 인증시스템을 구현 하면서, 토큰을 httpOnly 속성이 활성화된 쿠키에 담아서 구현을 하도록 하겠습니다.  
  
  
  
[Nest.js cookie based JWT authentication | Tigran.tech](https://tigran.tech/nestjs-cookie-based-jwt-authentication/)  
  
쿠키