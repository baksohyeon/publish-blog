---  
category: 질문 목록/Solved Problems/Concept  
share: true  
date created: Thursday, December 15th 2022, 1:16:32 pm  
date modified: Wednesday, January 25th 2023, 5:24:20 pm  
title: 세션 vs JWT  
---  
  
# 세션 vs JWT  
  
---  
  
## 질문 내용  
  
굳이 대형 서비스 아니고 중소규모 서비스면 jwt 붙일 필요가 잇나  
세션로그인이 낫지 않나? 어차피 20명정도만 써도 찢었다ㅋㅋ 이럴건데  
굳이 리프레시토큰 db 저장할거면(DB 덜 찌른다 차이일뿐인데)... 뭔 차이지  
  
서비스 사이즈 규모 및 보안성이나 앞으로 확장 여부에 따라 어떤 보안 방식을 택할 것인가 생각해야 할 필요 없이  
일단 로그인 구현 하고싶으면 걍 무지성으로 Jwt 쓰면 되는건가  
서비스를 만들 때 기준이 뭐임  
(왜냐면 뭔가 찾아볼수록 느낌이 jwt가 최신 기술에 검증되었으니 세션보다 무조건 좋은 방식일 것이다 하고 냅다 jwt 쓰는 것 처럼 느껴짐)  
  
서버 부하 온다 기준이 또 어느정도가 서버가 못견디는거지  
설명 찾다 보면 서버가 완전 개복치라서 1000명 정도 모여도 못견디는 것처럼 보임  
  
## 질문 관련 서치 내용  
  
### #1  
  
[Authentication: JWT usage vs session - Stack Overflow](https://stackoverflow.com/questions/43452896/authentication-jwt-usage-vs-session)  
[[인증/인가] 세션 / 토큰(JWT) 어떤걸 사용할까? — 일단은 내 이야기](https://kdhyo98.tistory.com/92)  
  
1. "Cheaper" authentication because you can eliminate a DB round trip (or at least have a much smaller table to query!), which in turns enable horizontal scalability.  
  
2. Tamper-proof client-side claims.  
  
3. it is reasonably easy to implement using libraries available in probably every language you can expect to have it.  
4. It is also completely divorced from your initial user authentication scheme  
  
   - if you move to a fingerprint-based system, you do not need to make any changes to the session management scheme.  
  
5. 세션 ID를 저장하는 쿠키 같은 경우 모바일과 원활하게 결합하지 않을 수 있지만, 토큰은 구현하기가 쉽다.  
  
6. OAuth 인증 방식이 많이 활용되면서 확장성이 높음  
7. With an in-memory solution, you limit your horizontal scaling, and sessions will be affected by network issues (clients roaming between Wifi and mobile data, servers rebooting, etc).  
  
### #2  
  
[Difference between Session Cookies vs. JWT (JSON Web Tokens), for session management. | by Prashant Ram | Medium](https://medium.com/@prashantramnyc/difference-between-session-cookies-vs-jwt-json-web-tokens-for-session-management-4be67d2f066e)  
  
**Session Cookies vs. JSON Web tokens — Advantages and Disadvantages**  
  
- `Session` Cookies approach:  
  <mark class="yellow">Advantages</mark>  
  
On logout, since the sessionId is removed from the database, we have a precision logout i.e. logout occurs at the moment the sessionId is removed from the sessionDB  
  
<mark class="yellow">Disadvantages</mark>  
Needs an additional DB lookup within the sessionId table to get the userId, DB look ups are relatively slower than JWT decrypting action.  
Individual servers cannot be scaled separately since they would need to share the sessionDB.  
  
- `JWT` (JSON Web token) approach:  
  <mark class="yellow">Advantages</mark>  
  
Since userId is got by decrypting the JWT token, no DB call is required to get userId, so somewhat faster that session approach.  
Servers can be scaled separately, without the need share sessionDB. This makes the JWT approach a great option for micro-services architecture.  
The same token can be used to authenticate on different servers (as long as server has the access_token_secret), without the need to share sessionDB,  
This also allows for a completely separate authentication server, that can be solely responsible to issue “accessTokens” and “refreshTokens”.  
  
<mark class="yellow">Disadvantages</mark>  
Since JWT tokens cannot be “invalidated” (without maintaining them in a shared db), in JWT approach the logout length precision is set by the expiration length of the access_token.  
However the “access_token” lifespan can be kept short (typically 10 to 15 mins), so that tokens are automatically “invalidated” after the duration.  
Anti-pattern: Sometimes additional and unnecessary information is stored in the JWT. The JWT token should primarily contain user information, and the data authorized to be accessed by that user should be provisioned and managed as a separate service on that respective server.  
  
### #3  
  
[Auth Headers vs JWT vs Sessions — How to Choose the Right Auth Technique for APIs | HackerNoon](https://hackernoon.com/auth-headers-vs-jwt-vs-sessions-how-to-choose-the-right-auth-technique-for-apis-57e15edd053)  
  
- There are two choices for Single Page Applications:  
  
1. Session Based  
2. Token-Based authentication  
  
The set of questions that need to be asked are:  
  
- **Should the sessions be invalidated before they expire?**  
  If Yes, Sessions must be preferred.  
  
- **Should the session end based on inactivity as against ending after a fixed time?**  
  If Yes, Sessions must be preferred.  
  
- **Should the me functionality be remembered?**  
  If Yes, Sessions must be preferred.  
- **Will mobile applications to use the same APIs?**  
  If yes, prefer token-based authentication (but ensure a separate API is built for these use cases)  
  
- **Is Your web framework protected against CSRF?**  
  Prefer token based authentication if it is a “No” or if you don’t know what CSRF is.  
  
Mobile apps do not automatically maintain and send session cookies. Hence it would be far easier for a mobile app developer to set an authentication token as opposed to setting a session cookie.  
  
- The three choices for APIs that are called from a server-side application includes:  
  
1. Basic authentication over https  
2. Token-based authentication  
3. Signature-based authentication  
  
- **Will the API be used only internally and only by the applications you control, are the API themselves of low value?**  
  If yes, as long as you are using HTTPS, basic authentication is acceptable.  
  
- **Would you want the keys to be long-lived?**  
  If yes, since the keys themselves are never sent over the wire, signature-based authentication.  
- **Would you want the same APIs to be used for both mobile and web apps?**  
  If yes, since signature based auth requires clients to store secrets, prefer token-based authentication.  
- **Are you using OAuth?**  
  If yes, the decision is made for you. OAuth 1 uses signature-based authentication, whereas OAuth 2 uses token-based authentication.  
- **Do you provide a client library to access your APIs?**  
  If yes, prefer signature based auth, because you can then write the cryptography code once and provide it to all your clients.  
  
## 질문을 정리하면서 이해한 것  
  
넷플릭스처럼 사용자를 제한하거나, 다른 디바이스에서 접속된 세션을 강제 로그아웃을 진행해야 하는 이유가 있을 땐 세션 방식을 사용  
  
단일 페이지인 경우 세션이 좀 더 효과적  
  
웹이 아닌 모바일이나 다른 클라이언트, 외부 API 통신 등 여러 곳에서 인가를 진행해야 할 때에는 토큰 방식이 효과적  
  
모바일 환경 등 환경 변화에 다양한거 대응 쉽고 다른 인증인가 (Oauth) 등에서 확장성 좋고, 라이브 서버 안켜도 되어서 싸고 또 구현 쉬워서 jwt 쓰는거고  
유저수 변동이 그렇게 크지 않고 환경이 다양하지 않고, 유저 빡빡하게 잡고 싶으면 세션 쓰긴 하는데 그게 아니면 안쓰는 느낌  
  
## 질문 답변 (해결 방안)  
  
세션 구현이 더 까다로움 라이브 서버도 필요하고... 세션 날라가면 어떡함  
인증인가에 대한 부담이 줄어서 씀  
  
jwt 가 쓰는게 내한테는 더 합당한 듯  
  
## 추가 질문  
  
1. 서버 부하 온다 기준이 또 어느정도가 서버가 못견디는 것인지..  
   설명 찾다 보면 서버가 완전 개복치라서 1000명 정도 모여도 못견디는 것처럼 보임  
  
2. DB 부하란게 그럼 정확히 뭐지  
   [[대용량DB] 대규모 웹 서비스란?](https://12bme.tistory.com/100)  
   [Horizontal Vs. Vertical Scaling: How Do They Compare?](https://www.cloudzero.com/blog/horizontal-vs-vertical-scaling)  
   [데이터베이스 Call과 네트워크 부하 – DATA ON-AIR](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?mod=document&uid=359)  
  
2번은... 보통 세션이나 JWT 스토어 같은 걸 구현한다고 치면 레디스를 쓰게 될 텐데  
세션의 경우에는 세션 키 - 세션 데이터의 매핑을 레디스에 넣고 IO하게 될 거고  
JWT의 경우에는 JWT 토큰을 걍 레디스에 expire 걸어서 넣어두는 식으로 구현이 될 텐데  
전자의 경우에는 아무래도 레디스에서 데이터를 읽어와야 하니 후자보다는 레디스가 할 일이 많겠죠?  
그리고 전자는 레디스를 갔다 와야 유저 정보를 알아내고 따라서 본격적으로 뭔가 시작할 수 있는데  
후자는 일단 레디스에 확인 요청만 보내놓고 서버 작업은 해! 근데 이것저것 작업 다 하고 API 요청 응답 보내기 전에 레디스에서 온 응답 확인했는데 토큰이 만료됐대! 그럼 이때까지 한 거 다 무효! 같은 식으로 작업을 시작하는 타이밍을 당기고 결과를 확정하기 전에만 레디스 요청 결과를 활용함으로써 최적화를 할 수 있어요  
(물론 보통은 이렇게까지는 안 하는 것 같아요 😂 이론상 그렇다는 얘기....)  
  
그래서 사실 이 최적화는 비인증 요청에 대해서 DB 작업을 섣불리 시작한다는 점에서 오히려 DB 로드를 증가시키고 디도스 등의 과부하 공격의 가능성을 열어 두는 최적화이기도 해요  
결국 상황 따라서 장단점을 고려하고 상황에 맞는 최적의 솔루션을 내야 한다는 점!  
  
근데 사실 노드 기준에선 프로미스 다루는 게 워낙 편하다 보니 못할 이유도 없는 구현이긴 해요  
디비 트랜잭션 맨 마지막에만 레디스 응답 체크하고 commit/rollback 결정하면 되니...  
언어마다도 다르고.... (일례로 Rust 백엔드는 Node 백엔드보다 일반적으로 10배 더 많은 요청을 처리할 수 있음)  
그거가 있을 수가 없는 게 로직은 사람이 짜는 것이기 때문에....  
그나마 그걸 재려면 서버 벤치마크 같은 걸 확인해보면 되는데요  
https://www.techempower.com/benchmarks/#section=data-r21  
  
여기 보시면 일단 테스트가 여러 종류 있고 (JSON Serialization, Single query, Multiple queries, Cached queries, Fortunes, Data updates, Plaintext) 이 테스트별로 각각 다양한 언어의 프레임워크들로 구현된 벤치마크들이 존재하는데  
대충 어떤 언어의 어떤 프레임워크가 어떤 종류 작업에 강점을 가지는지? 같은 거 참고하기 좋아요  
아마 Multiple queries가 가장 참고하기 좋은 대상이실 듯  
아 아니겠다! Fortunes 보시면 될듯요  
  
Requirements summary  
  
```  
In this test, the framework's ORM is used to fetch all rows from a database table containing an unknown number of Unix fortune cookie messages (the table has 12 rows, but the code cannot have foreknowledge of the table's size). An additional fortune cookie message is inserted into the list at runtime and then the list is sorted by the message text. Finally, the list is delivered to the client using a server-side HTML template. The message text must be considered untrusted and properly escaped and the UTF-8 fortune messages must be rendered properly.  
```  
  
이게 Fortunes 벤치마크 설명인데  
대충 일반적인 서버가 할 만한 일을 가장 잘 표현한 벤치마크인듯  
근데 일반적으로 Stateless한 건 Horizontal Scaling (서버 장비 추가를 통한 스케일링) 이 가능한데  
Stateful한 건 Vertical Scaling밖에 못 해요 (프로덕션 서비스에서 DB 인스턴스 스펙 오지게 키우는 이유가 이것)  
물론 이것을 완화하기 위해.... 샤딩이라거나 (테이블/Row 범위별로 담당하는 DB 서버를 분리) 레플리카라던가 (Read를 Write보다 많이 할 테니 Read-only 서버를 여러 대 만들어서 Write 지원 서버에서 쓰기가 일어날 때마다 해당 내용을 모두 Read-only 서버에 복사하고 읽기 쿼리는 Read-only 서버 대상으로만 실행) 등의 처리를 하는데 모두 한계가 있고요  
  
물론 요즘은 뭐 MongoDB니 CockroachDB니 Planetscale이니 등등 스케일링 잘 되는 DB들도 있긴 한데.... 암튼 그래서 보통 웹 애플리케이션의 성능에 대해 이야기할 때 DB가 병목이다! 라고들 이야기를 많이 하고요 (특히 DB는 하드디스크/SSD에서 데이터를 읽기 때문에 메모리에서 읽는 것보다 성능이 떨어지며, 툭하면 트랜잭션 때문에 락 걸려서 동시성도 떨어짐)  
그래서 DB 요청을 최적화하는 것이 오랜 기간 동안 웹 애플리케이션 성능 개선에 중요한 포인트였어요  
  
오 그러면 요청 빈도수 보다 얼마나 퀄리티 있게 요청하느냐가 더 중요한 것인지..?  
  
아 그거는 DB의 Explain에 대해서 검색ㄱㄱ  
