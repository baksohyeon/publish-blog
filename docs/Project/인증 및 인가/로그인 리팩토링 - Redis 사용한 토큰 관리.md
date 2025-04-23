---
category: Project/인증 및 인가
share: true
---
  
# 리팩토링: Redis 로 Token 관리하기 : 블랙리스트? 화이트 리스트?  
  
- node.js 기반으로 한 코드를 기반으로 nest js 프로젝트에 적용해보려 한다.  
  [TheKenyanDev: Access & Refresh Tokens - A Deep Dive into the JWT Authentication Flow By Building an Authentication System with NodeJS & Redis](https://thekenyandev.com/blog/jwt-access-&-refresh-tokens-building-a-somewhat-stateless-jwt-authentication-system-with-nodejs-redis-and-react-part-one-server/)  
  
[Better approach for Jwt authentication and session management - Stack Overflow](https://stackoverflow.com/questions/71201282/better-approach-for-jwt-authentication-and-session-management) 에 따르면 the refresh token along with the access token is made with uuid then stored in Redis for session and in this refresh token has a expiration and it is changed when new access token is requested.  
  
  
[node.js - Should I store JWT tokens in redis? - Stack Overflow](https://stackoverflow.com/questions/44890564/should-i-store-jwt-tokens-in-redis) 에 따르면 유효하지 않은 토큰들을 black list 에 올려서 관리한다고 한다.  
  
TLDR: 토큰을 취소할 수 있는 기능을 원한다면, 예, 레디스와 같은 빠른 제품에 저장하십시오.  
  
JWT를 사용할 때 단점 중 하나는 사용자가 로그아웃해야 하거나 토큰이 손상된 경우 토큰을 취소할 수 있는 간단한 방법이 없다는 것입니다.  
토큰을 취소하는 것은 일부 저장소에서 해당 토큰을 검색한 후 다음에 수행할 작업을 결정하는 것을 의미합니다.   
JWT의 요점 중 하나가 db로의 왕복 여행을 피하는 것이기 때문에, 좋은 절충안은 rdbm보다 덜 부담되는 것에 보관하는 것이다. 그건 레디스에게 완벽한 솔루션이다.  
  
주석에서 제안한 것처럼 목록을 블랙리스트(즉, 무효화된 토큰 목록)로 만드는 것이 좋습니다. 요청할 때마다 목록을 검색하여 토큰이 목록에 없는지 확인합니다.   
  
확률적 알고리즘을 사용하여 토큰을 저장하면 조회 단계 중에 메모리 공간과 성능을 더욱 향상시킬 수 있습니다. 간단한 접근법은 계층화된 룩업을 갖는 것이다.  
  
예를 들어 블랙리스트에 있는 토큰의 처음 몇 바이트(예: 1~4바이트)만 추적하는 소규모 앱 스토어가 있을 수 있습니다.   
그런 다음 레디스 캐시는 동일한 토큰의 약간 더 완전한 버전(예: 처음 2~8바이트)을 추적합니다. 그런 다음 보다 영구적인 솔루션(파일 시스템, rdbms 등)을 사용하여 블랙리스트에 있는 토큰의 전체 버전을 저장할 수 있습니다.   
이는 토큰이 블랙리스트에 없음을 신속하게 확인하는 낙관적인 조회 전략입니다(일반적인 경우). 검색 중인 토큰이 앱 내 블랙리스트의 항목과 일치하는 경우(처음 몇 바이트가 일치하기 때문에), 다시 저장소에 대한 추가 조회로 이동하고 필요한 경우 영구 저장소로 이동합니다.   
  
스토어들의 일부(또는 전부)는 트라이 또는 해시 테이블로 구현될 수 있다. 고려해야 할 또 다른 효율적이고 비교적 간단한 데이터 구조는 [블룸 필터](https://en.wikipedia.org/wiki/Bloom_filter)라고 불리는 것이다.  
  
수백만 개의 오래 지속되는 토큰을 정기적으로 블랙리스트에 올린다면 분명히 위의 접근 방식을 적용해야 합니다(다른 문제가 있음을 나타낼 수도 있음).  
  
→ block list 관련 토큰 다룰 때 참고할 로직: [여기](https://flask-jwt-extended.readthedocs.io/en/latest/blocklist_and_token_revoking/)서 참고해서 써보자.  
  
  
[출처](https://stackoverflow.com/questions/52617942/how-to-use-a-jwt-refresh-token-to-generate-a-new-access-token) I'm assuming that you have your own back-end to handle the refresh token process. Please tell me if this is not the case  
  
What I did to this process is to move all decoding and encoding to the back-end. But you have to make sure that you store the latest active refresh token in the back-end. Otherwise, someone could reuse old token to create access token.  
  
In the front-end store the expiry date. Then, everytime you make a request to the back-end, check if the expiry date is not exceeded (probably you want to take into account delays of the request e.g. 5 seconds before expiry). If it's expired, fire the refresh-token method.  
Create a refresh token endpoint in the back-end and send both access-token and refresh-token to it  
Decode the access-token and get your necessary data. Ignore expiry date in this decode function.  
Compare refresh-token with the latest refresh-token in the db. If it doesn't match, the user is not authorized. Otherwise, continue.  
Now if you want to reuse the old data, you don't need to query your database and just re-encode the access-token content to a new token. Otherwise, do your query and rebuild the access-token.  
  
  
  
- 블랙리스트 vs 화이트리스트 저장   
  [web services - Blacklist JWT tokens or whitelist JWT tokens - Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/429742/blacklist-jwt-tokens-or-whitelist-jwt-tokens)  
요약하자면 "블랙리스트는 명시적으로 로그아웃한 애들만 저장하면 되는 반면, 화이트리스트는 로그인한 애들을 전부 들고있어야 하기 때문에 블랙리스트의 크기가 더 작을 수밖에 없고 그래서 이쪽이 더 낫다"는 이야기인데  
이건 철저히 액세스 토큰에 국한된 설명  
  
리프레시 토큰 7일로 잡아놔서   
블랙리스트 vs 화이트리스트가 기준이 둘 중 어떤게 크기가 더 작을 것이냐!   
블랙리스트가 크기가 커지는 경우는 "수명이 다하기 전에 비활성화된 토큰의 수가 수명이 남아 있는 활성 토큰의 수보다 많은 경우"고  
화이트리스트는 정확히 그 반대인데 7일 정도면 블랙리스트 쪽이 크기가 훨씬 작을 것 같음! ^e34c97  
  
## 기존 코드  
- google.strategy.ts  
```ts  
import { Inject, Injectable } from '@nestjs/common';  
import { ConfigType } from '@nestjs/config';  
import { PassportStrategy } from '@nestjs/passport';  
import { Profile, Strategy, VerifyCallback } from 'passport-google-oauth20';  
import { googleConfiguration } from 'src/config/google.config';  
import { UserService } from 'src/user/user.service';  
import { AuthService } from '../auth.service';  
import { LoginRequestUserDto } from '../dto/login-request.dto';  
// import { UserInfoDto } from '../dto/user-info.dto';  
  
@Injectable()  
export class GoogleStrategy extends PassportStrategy(Strategy, 'google') {  
  constructor(  
    @Inject(googleConfiguration.KEY)  
    private readonly googleConfig: ConfigType<typeof googleConfiguration>,  
    private readonly authService: AuthService,  
    private readonly userService: UserService,  
  ) {  
    super({  
      clientID: googleConfig.clientId,  
      clientSecret: googleConfig.clientSecret,  
      callbackURL: googleConfig.callbackURL,  
      scope: ['profile', 'email'],  
    });  
  }  
  async validate(  
    _accessToken: string,  
    _refreshToken: string,  
    profile: Profile,  
    done: VerifyCallback,  
  ): Promise<any> {  
    const { name, emails } = profile;  
    const userInfo = {  
      email: emails[0].value,  
      username: `${name.familyName} ${name.givenName}`,  
    } as LoginRequestUserDto;  
	// 1. 유저 refresh 토큰 빼고 db에 저장   
    const user = await this.userService.findUserByEmail(userInfo.email);  
    if (!user) {  
      const newUser = await this.userService.createUser(userInfo);  
      done(null, newUser);  
    }  
    done(null, user);  
  }  
}  
  
```  
  
- src/auth/auth.controller.ts  
```ts  
  
  @UseGuards(GoogleOauthGaurd)  
  @Get('google/login')  
  handleLogin() {  
    // Guard redirects  
  }  
  
  @UseGuards(GoogleOauthGaurd)  
  @Get('google/redirect')  
  async googleAuthCallback(  
    @RequestUser() user: User,  
    @Res({ passthrough: true }) res: Response,  
  ) {  
    try {  
      const accessCookieConfig =  
        await this.authService.getAccessTokenCookieConfig(user);  
      const refreshCookieConfig =  
        await this.authService.getRefreshTokenCookieConfig(user);  
	  // 리다이렉트 성공하면 해시된 refresh 토큰 유저 db 저장  
      this.authService.saveHashedRefreshToken(  
        refreshCookieConfig.refreshToken,  
        user.id,  
      );  
  
      res.cookie(  
        'access_token',  
        accessCookieConfig.accessToken,  
        accessCookieConfig.accessCookieOptions,  
      );  
      res.cookie(  
        'refresh_token',  
        refreshCookieConfig.refreshToken,  
        refreshCookieConfig.refreshCookieOptions,  
      );  
      res.redirect(this.configService.get<string>('FRONTEND_URL'));  
    } catch (e: any) {  
      this.logger.error(e.message);  
      throw new InternalServerErrorException(e.message ?? 'error has occurred');  
    }  
  }  
  
```  
  
유저 저장 (refresh token = null) 이후 따로 update 시키면 트랜잭션 보장이 안되어서 불안하다.   
refresh 토큰을 더 낫게 사용하는 방법에 대해서 구글링해보았더니 레디스를 활용한 방법이 있다고 한다.  
  
  
  
[node.js - Should I store JWT tokens in redis? - Stack Overflow](https://stackoverflow.com/questions/44890564/should-i-store-jwt-tokens-in-redis)  
  
  
  
- **Refresh Token의 저장소로 레디스를 선택한 이유**  
1. 빠른 액세스 속도로 사용자 로그인시 (리프레시 토큰 발급 시) 병목이 되지 않는다.  
2. 토큰을 취소할 수 있다.  
   사용자가 로그아웃 하거나 토큰이 손상된 경우 토큰을 취소할 수 있는 방법이 없다.  
   토큰을 취소한다는 것은 DB를 찔러야하기때문에... RDBM보다 덜 부담되는 redis에 보관한다고 한다.  
3. We use it to avoid repeated requests to our database. Other in-memory databases work fine, too, of course.  
  
  
- **선택하지 말아야 할 이유**  
 1. 고작 이거 하자고 레디스를 새로 띄우는 게 맞는가?   
 2. 레디스가 빠르다고 레디스로 이것저것 다 하려고 하면 오히려 레디스가 병목이 될 우려  
  
→ 일단 토큰 관리에 대해서 더 자세히 알아보고 싶고, 토큰 관리 면에서 현재 상태보다 더 나은 것 같아서 redis를 써서 해결하고자 한다.  
  
  
또 추가적으로 찾아보니 [jwt-redis - npm](https://www.npmjs.com/package/jwt-redis)  node.js 라이브러리 중 이런 게 있다.  
- 라이브러리 설명  
> Jwt-redis allows you to store the token label in redis to verify validity. The absence of a token label in redis makes the token not valid. To destroy the token in jwt-redis, there is a destroy method. This makes it possible to make a token not valid until it expires. Jwt-redis support node_redis client.  
>   
  
## What is Redis (Remote Dictionary Server)  
[[NestJS] DB 캐시 Redis 사용하기](https://tre2man.tistory.com/277)  
[What is Redis? - YouTube](https://www.youtube.com/watch?v=muqiHwa8UvQ&ab_channel=IBMTechnology)   
[2019-01-08, KISTI, Yeonghun Chae.pptx]([https://pjp1-powerpoint.officeapps.live.com/p/pdfhandler.ashx?PV=0&Pid=WOPIsrc%3Dhttp%253A%252F%252Fpjp1%252Dview%252Dwopi%252Ewopi%252Eonline%252Eoffice%252Enet%253A808%252Foh%252Fwopi%252Ffiles%252F%2540%252FwFileId%253FwFileId%253Dhttps%25253A%25252F%25252Fproinlab%25252Ecom%25253A443%25252Fpublish%25252F2019%25252F20190108%25252Epptx%26access_token%3D1%26access_token_ttl%3D0%26z%3Dc082e8d581993f3d25acb168642e10546f836be288d6ab9600554dd1b1593129&useNamedAction=1&waccluster=PJP1](https://pjp1-powerpoint.officeapps.live.com/p/pdfhandler.ashx?PV=0&Pid=WOPIsrc%3Dhttp%253A%252F%252Fpjp1%252Dview%252Dwopi%252Ewopi%252Eonline%252Eoffice%252Enet%253A808%252Foh%252Fwopi%252Ffiles%252F%2540%252FwFileId%253FwFileId%253Dhttps%25253A%25252F%25252Fproinlab%25252Ecom%25253A443%25252Fpublish%25252F2019%25252F20190108%25252Epptx%26access%5Ftoken%3D1%26access%5Ftoken%5Fttl%3D0%26z%3Dc082e8d581993f3d25acb168642e10546f836be288d6ab9600554dd1b1593129&useNamedAction=1&waccluster=PJP1))  
[Redis](https://velog.io/@top1506/Redis) // 나중에 더 읽어보기  
  
  
인메모리(RAM) 상태에서 데이터를 처리하여 다른 DB들보다 빠르고 가볍다.  
  
<mark class="yellow">Redis 특징</mark>  
▪ 클러스터 지원  
▪ Key-Value 데이터 모델   
  
▪ Publish-Subscribe Pattern 지원  
▪ 데이터에 대한 만료 시간 설정 가능  
▪ 메모리 데이터에 대한 백업 복구 기능  
  
<mark class="yellow">Key-Value Storage</mark>  
▪ 특정 키 값에 값을 저장하는 구조  
▪ Put / Get 을 통해 데이터 접근  
▪ 쉽게 생각하면 HashMap과 같은 형태의 데이터베이스  
  
<mark class="yellow">Redis vs Memcached</mark>  
▪ Memcached  
▪ 구조가 단순해서 Redis에 비해 적은 메모리 사용  
▪ String 데이터 타입만 지원함  
▪ 주로 HTML 캐싱에 사용됨  
▪ 서버가 재실행되면 데이터가 삭제됨 (메모리만 사용하여 데이터 관리)  
▪ Redis  
▪ 다양한 데이터 타입 지원  
▪ Pub-Sub 등 다양한 기능 지원  
▪ Session 관리, Message Queuing 등 다양한 응용  
▪ 서버가 재실행되더라도 데이터가 보존됨 (디스크 활용)  
  
  
  
<mark class="yellow">Redis Failover</mark>  
▪ Snapshotting 방식  
▪ 순간적으로 메모리에 있는 내용을 디스크로 백업  
▪ 백업을 할 때 시간이 오래 걸림  
▪ 주기적으로 백업하기 때문에 예기치 못한 오류 발생시 일부 데이터가 유실될 위험  
▪ AOF (Append On File) 방식  
▪ Get, Put 등 모든 상태 변화가 발생할 때 상태를 디스크에 기록함  
▪ 상태 변화에 따른 로그를 파일로 저장하여 순차적으로 실행하는 방식  
▪ 재실행 시 속도가 느림  
  
<mark class="yellow">Pub-Sub 모델</mark>  
▪ 쉽게 생각하면 채팅  
▪ 특정 클라이언트가 Redis로 Publish(데이터 추가)를 하면 Subscribe 하고있는 모든 클라이언트에 전달 (Broadcast)  
▪ Pub-Sub을 활용하여 Message Queuing  시스템으로 사용 가능  
![Pasted image 20230110174517.png](../../attachments/Pasted%20image%2020230110174517.png)  
  
  
<mark class="yellow">Redis 사용시 주의사항</mark>  
▪ 메모리 용량을 초과하는 데이터를 보관  
▪ Disk Swap 영역을 사용하게 되기 때문에 속도가 느려짐  
▪ 기본적으로 자주 사용되는 메모리가 우선적으로 메모리에 남아있기 때문에 사용시 큰 문  
제가 되지는 않지만, 과도하게 용량이 초과되는 경우에는 조치가 필요함  
▪ 충분한 메모리 확보가 필요  
▪ 데이터 백업  
▪ 데이터에 대해 100% 안전하게 보관이 불가능하기 때문에 휘발성 데이터에 대해서만 주  
로 사용 권장  
▪ 보안  
▪ 기본적으로 별도의 보안 옵션을 제공하지 않기 때문에 방화벽 설정이 필요함  
[Fetching Title#s0hy](https://yeonyeon.tistory.com/164)  
  
<mark class="yellow">Redis 활용 예시</mark>  
▪ 웹 서버에서 HTML 캐싱  
▪ 사용자 로그인 Session 관리  
▪ 채팅 어플리케이션 서버  
▪ Message Broker 시스템  
  
KV DB. Data in Reids memory can be stored on disk: RDB and AOF can get the snapshots and edit logs.  
Message Queue. But one message can only be consumed by one consumer  
Pubsub  
Distributed lock. Rely on the setnx command, and only the first thread executing it successfully will hold the lock. https://redis.io/commands/setnx  
  
갱신 과정은  
1. Refresh Token이 유효한지 검증합니다.  
2. Access Token에서 Authentication 객체를 가지고 와서 저장된 name(email)을 가지고 옵니다.  
3. email을 가지고 Redis에 저장된 Refresh Token을 가지고 와서 입력받은 Refresh Token 값과 비교합니다.  
4. Authentication 객체를 가지고 새로운 토큰을 생성합니다.  
5. Redis에 새로 생성된 Refresh Token을 저장합니다.  
6. 클라이언트에게 새로 발급된 토큰 정보를 내려줍니다.  
  
[Usages of Redis](https://stackoverflow.com/a/67408695/20846217)  
  
1. Cache with multiple data structures, like: string, set, zset, list, hash and bitmap (which could be used in many aggregation use cases)  
2. KV DB. Data in Reids memory can be stored on disk: RDB and AOF can get the snapshots and edit logs.  
3. Message Queue. But one message can only be consumed by one consumer  
4. Pubsub  
5. Distributed lock. Rely on the setnx command, and only the first thread executing it successfully will hold the lock. https://redis.io/commands/setnx  
## OWASP  
[Authentication - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#authentication-and-error-messages)  
오오.. 보안 관련 참고하면 좋은 유명한 사이트  
  
The same message is sent when a password is incorrect, as when an account doesn't exist   
  
A discerning attacker could notice that different messages are sent depending on whether a username exits in the database or not. Brute-forcing into an account they know exists is much easier than guessing both the username and password.  
  
  
  
  
They are declared in the JWT spec and serve as a good starting point for useful, interoperable claims. These are normally generated automatically by JWT libraries and cannot be re-declared by an application.  
  
Their roles are:  
  
iss: The issuer. Who issued this token?  
aud: The audience. Who is this token intended for?  
iat: When the token was issued.  
exp: When the token will expire.  
jti: A unique identifier for the token.  
  
## Token Claims  
A rundown of each of these keys is as follows:  
  
```json  
{  
  "userId": "6a034d6f-f9bd-442c-afe9-888cd60760c1",  
  "role": "ROLE_USER",  
  "isVerified": true,  
  "tokenType": "access",  
  "refreshTokenId": "443d8502-eddb-4138-bee2-7a46cb5541d5",  
  "iss": "api.retrobie.com",  
  "aud": ["retrobie.com"],  
  "iat": 1516239022,  
  "exp": 1516239022,  
  "jti": "86dc2e6d-d964-4022-915b-9bd2ec01df14"  
}  
```  
  
**userId**: clients can only access the API with tokens generated for them.  
**role**: certain actions can only be performed by users with the ROLE_ADMIN role.  
**isVerified**: certain actions can only be performed by users with verified accounts.  
**tokenType**: used to distinguish between different types of tokens that we will generate.  
**refreshTokenId**: helps us to identify the refresh token that the access token was created alongside.  
  
JWT spec 설명  
`iss`: The issuer. Who issued this token?  
`aud`: The audience. Who is this token intended for?  
`iat`: When the token was issued.  
`exp`: When the token will expire.  
`jti`: A <font style="color:indianred">unique identifier</font> for the token.  
  
  
And the claims for our refresh tokens (excluding registered claim names):  
```json  
  
{  
  "userId": "6a034d6f-f9bd-442c-afe9-888cd60760c1",  
  "tokenType": "refresh",  
  "jti": "443d8502-eddb-4138-bee2-7a46cb5541d5"  
}  
```  
  
  
ms 라이브러리 사용함 (util 대체)  
  
  
uuid로 jti 추가함  
[3 efficient ways to generate UUID in Node.js](https://geshan.com.np/blog/2022/01/nodejs-uuid/)보니까 uuid 라이브러리가 압도적으로 다운로드 수가 많아서 uuid 패키지 설치할거임  
![Pasted image 20230109183746.png](../../attachments/Pasted%20image%2020230109183746.png)  
  
[uuid - npm](https://www.npmjs.com/package/uuid) 이 패키지 설치함  
  
  
  
---  
  
# Access Token Strategy 수정  
[passport-jwt/lib at master · mikenicholson/passport-jwt · GitHub](https://github.com/mikenicholson/passport-jwt/tree/master/lib)라이브러리 보면  
```ts  
// Load modules.  
var OAuth2Strategy = require('passport-oauth2')  
  , util = require('util')  
  , uri = require('url')  
  , GooglePlusProfile = require('./profile/googleplus')  
  , OpenIDProfile = require('./profile/openid')  
  , InternalOAuthError = require('passport-oauth2').InternalOAuthError  
  , GooglePlusAPIError = require('./errors/googleplusapierror')  
  , UserInfoError = require('./errors/userinfoerror');  
  
  
/**  
 * `Strategy` constructor.  
 *  
 * The Google authentication strategy authenticates requests by delegating to  
 * Google using the OAuth 2.0 protocol.  
 *  
 * Applications must supply a `verify` callback which accepts an `accessToken`,  
 * `refreshToken` and service-specific `profile`, and then calls the `cb`  
 * callback supplying a `user`, which should be set to `false` if the  
 * credentials are not valid.  If an exception occured, `err` should be set.  
 *  
 * Options:  
 *   - `clientID`      your Google application's client id  
 *   - `clientSecret`  your Google application's client secret  
 *   - `callbackURL`   URL to which Google will redirect the user after granting authorization  
 *  
 * Examples:  
 *  
 *     passport.use(new GoogleStrategy({  
 *         clientID: '123-456-789',  
 *         clientSecret: 'shhh-its-a-secret'  
 *         callbackURL: 'https://www.example.net/auth/google/callback'  
 *       },  
 *       function(accessToken, refreshToken, profile, cb) {  
 *         User.findOrCreate(..., function (err, user) {  
 *           cb(err, user);  
 *         });  
 *       }  
 *     ));  
 *  
 * @constructor  
 * @param {object} options  
 * @param {function} verify  
 * @access public  
 */  
function Strategy(options, verify) {  
  options = options || {};  
  options.authorizationURL = options.authorizationURL || 'https://accounts.google.com/o/oauth2/v2/auth';  
  options.tokenURL = options.tokenURL || 'https://www.googleapis.com/oauth2/v4/token';  
  
  OAuth2Strategy.call(this, options, verify);  
  this.name = 'google';  
  this._userProfileURL = options.userProfileURL || 'https://www.googleapis.com/oauth2/v3/userinfo';  
    
  var url = uri.parse(this._userProfileURL);  
  if (url.pathname.indexOf('/userinfo') == (url.pathname.length - '/userinfo'.length)) {  
    this._userProfileFormat = 'openid';  
  } else {  
    this._userProfileFormat = 'google+'; // Google Sign-In  
  }  
}  
  
// Inherit from `OAuth2Strategy`.  
util.inherits(Strategy, OAuth2Strategy);  
  
  
/**  
 * Retrieve user profile from Google.  
 *  
 * This function constructs a normalized profile, with the following properties:  
 *  
 *   - `provider`         always set to `google`  
 *   - `id`  
 *   - `username`  
 *   - `displayName`  
 *  
 * @param {string} accessToken  
 * @param {function} done  
 * @access protected  
 */  
Strategy.prototype.userProfile = function(accessToken, done) {  
  var self = this;  
  this._oauth2.get(this._userProfileURL, accessToken, function (err, body, res) {  
    var json;  
      
    if (err) {  
      if (err.data) {  
        try {  
          json = JSON.parse(err.data);  
        } catch (_) {}  
      }  
        
      if (json && json.error && json.error.message) {  
        return done(new GooglePlusAPIError(json.error.message, json.error.code));  
      } else if (json && json.error && json.error_description) {  
        return done(new UserInfoError(json.error_description, json.error));  
      }  
      return done(new InternalOAuthError('Failed to fetch user profile', err));  
    }  
      
    try {  
      json = JSON.parse(body);  
    } catch (ex) {  
      return done(new Error('Failed to parse user profile'));  
    }  
      
    var profile;  
    switch (self._userProfileFormat) {  
    case 'openid':  
      profile = OpenIDProfile.parse(json);  
      break;  
    default: // Google Sign-In  
      profile = GooglePlusProfile.parse(json);  
      break;  
    }  
      
    profile.provider  = 'google';  
    profile._raw = body;  
    profile._json = json;  
      
    done(null, profile);  
  });  
}  
  
/**  
 * Return extra Google-specific parameters to be included in the authorization  
 * request.  
 *  
 * @param {object} options  
 * @return {object}  
 * @access protected  
 */  
Strategy.prototype.authorizationParams = function(options) {  
  var params = {};  
    
  // https://developers.google.com/identity/protocols/OAuth2WebServer  
  if (options.accessType) {  
    params['access_type'] = options.accessType;  
  }  
  if (options.prompt) {  
    params['prompt'] = options.prompt;  
  }  
  if (options.loginHint) {  
    params['login_hint'] = options.loginHint;  
  }  
  if (options.includeGrantedScopes) {  
    params['include_granted_scopes'] = true;  
  }  
    
  // https://developers.google.com/identity/protocols/OpenIDConnect  
  if (options.display) {  
    // Specify what kind of display consent screen to display to users.  
    //   https://developers.google.com/accounts/docs/OpenIDConnect#authenticationuriparameters  
    params['display'] = options.display;  
  }  
    
  // Google Apps for Work  
  if (options.hostedDomain || options.hd) {  
    // This parameter is derived from Google's OAuth 1.0 endpoint, and (although  
    // undocumented) is supported by Google's OAuth 2.0 endpoint was well.  
    //   https://developers.google.com/accounts/docs/OAuth_ref  
    params['hd'] = options.hostedDomain || options.hd;  
  }  
    
  // Google+  
  if (options.requestVisibleActions) {  
    // Space separated list of allowed app actions  
    // as documented at:  
    //  https://developers.google.com/+/web/app-activities/#writing_an_app_activity_using_the_google_apis_client_libraries  
    //  https://developers.google.com/+/api/moment-types/  
    params['request_visible_actions'] = options.requestVisibleActions;  
  }  
    
  // OpenID 2.0 migration  
  if (options.openIDRealm) {  
    // This parameter is needed when migrating users from Google's OpenID 2.0 to OAuth 2.0  
    //   https://developers.google.com/accounts/docs/OpenID?hl=ja#adjust-uri  
    params['openid.realm'] = options.openIDRealm;  
  }  
    
  // Undocumented  
  if (options.approvalPrompt) {  
    params['approval_prompt'] = options.approvalPrompt;  
  }  
  if (options.userID) {  
    // Undocumented, but supported by Google's OAuth 2.0 endpoint.  Appears to  
    // be equivalent to `login_hint`.  
    params['user_id'] = options.userID;  
  }  
    
  return params;  
}  
  
  
/**  
 * Expose `Strategy`.  
 */  
module.exports = Strategy;  
```  
  
이렇게 구현해둠.   
node js 랑 비교하면서 보자.  
  
## Passport 모듈  
[[NODE] 📚 Passport 모듈 사용법 (그림으로 처리 과정 💯 이해하기)](https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-Passport-%EB%AA%A8%EB%93%88-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%B2%98%EB%A6%AC%EA%B3%BC%EC%A0%95-%F0%9F%92%AF-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90)  
  
```js  
// jwt 정규식  
const regex = ^[\w-]*\.[\w-]*\.[\w-]*$   
```  
  
  
The refresh token should be passed as a <font style="color:slateblue">secure</font> (in production), <font style="color:slateblue">HttpOnly</font> cookie.   
As long as the correct path is defined in the cookie, the browser will automatically send it together with any requests made to that endpoint  
  
  
---  
  
- Refreshing Expired Access Tokens  
We first decode the refresh token and fetch its state from Redis.  
  
  
If it's present in Redis and is active, we then check if the refresh token has the same id as the refreshTokenId field in the decoded access token.  
  
  
# Refresh, Access Token 로그인 구현하기  
<font style="color:indianred">//WIP</font>  
# 로직 수정  
  
iss (Issuer) : 토큰 발급자  
sub (Subject) : 토큰 제목 - 토큰에서 사용자에 대한 식별값이 됨  
aud (Audience) : 토큰 대상자  
exp (Expiration Time) : 토큰 만료 시간  
nbf (Not Before) : 토큰 활성 날짜 (이 날짜 이전의 토큰은 활성화 되지 않음을 보장)  
iat (Issued At) : 토큰 발급 시간  
jti (JWT Id) : JWT 토큰 식별자 (issuer가 여러명일 때 이를 구분하기 위한 값)  
  
  
# Payload  
- login 이후 반환되는 payload 값  
```json  
{  
	"user": {  
		"id": "f0c9ad9e-8e85-11ed-93a9-de361dafd48a",  
		"email": "bori12370@gmail.com",  
		"username": "박 소현",  
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
  
- decoded access token  
```  
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImJvcmkxMjM3MEBnbWFpbC5jb20iLCJ1c2VybmFtZSI6IuuwlSDshoztmIQiLCJpc1ZlcmlmaWVkIjpmYWxzZSwidXNlcklkIjoiZjBjOWFkOWUtOGU4NS0xMWVkLTkzYTktZGUzNjFkYWZkNDhhIiwicmVmcmVzaFRva2VuSWQiOiI2ZTRkNTIyNC0yMGFlLTQyMGYtYTRjMS03ZmUwYjllYjNiMzEiLCJ0b2tlblR5cGUiOiJhY2Nlc3MiLCJpYXQiOjE2NzM0MjU3OTAsImV4cCI6MTY3NTIyNTc5MCwiYXVkIjpbImh0dHA6Ly9sb2NhbGhvc3Q6MzAwMCJdLCJpc3MiOiJkb3JpdG8iLCJqdGkiOiIzNmU0NzAzNC1mODE0LTQxY2YtODg4My0wNTNjM2JhZTNiOGEifQ.YgTrRsDk7ZEIpeFAspHnG-2u3xvYyD-nIaeDTuAdH0g  
```  
  
  
```json  
{  
  "email": "bori12370@gmail.com",  
  "username": "박 소현",  
  "isVerified": false, // isValid 로 수정하기  
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
  
- decoded refresh token  
```  
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiJmMGM5YWQ5ZS04ZTg1LTExZWQtOTNhOS1kZTM2MWRhZmQ0OGEiLCJ0b2tlblR5cGUiOiJyZWZyZXNoIiwiaWF0IjoxNjczNDI1NzkwLCJleHAiOjIyNzgyMjU3OTAsImF1ZCI6WyJodHRwOi8vbG9jYWxob3N0OjMwMDAiXSwiaXNzIjoiZG9yaXRvIiwianRpIjoiNmU0ZDUyMjQtMjBhZS00MjBmLWE0YzEtN2ZlMGI5ZWIzYjMxIn0.MuFVtVx0GYNFozWg-6PFQgjRzCcDAqC31W01_ZHCkNs  
```  
  
  
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
  
# NestJs Caching With Redis  
- 공식 문서  
[Redis - Microservices | NestJS - A progressive Node.js framework](https://docs.nestjs.com/microservices/redis#redis)  
[Caching | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/caching#caching)  
[레디스 공식 문서](https://redis.io/docs/about/)  
- 참고 블로그  
[NestJs Caching With Redis - Code with Vlad](https://www.codewithvlad.com/blog/nestjs-caching-with-redis)   
## 개념 정리  
### Caching 이란?  
![Pasted image 20230109223233.png](../../attachments/Pasted%20image%2020230109223233.png)  
  
Caching involves saving frequently requested data in an intermediary store called the "**cache store**" to avoid unnecessary calls to the primary database.  
  
An HTTP request asking for data cached by the server will receive it directly from the cache store instead of getting it from a database. Which is much faster!  
  
  
Since most relational databases involve structured data, they are optimised for **reliability and not for speed**. That means the data they store on a disk is many times slower than the RAM. Using a NoSQL database does not bring any tremendous performance gains either.  
  
**The solution is to use an in-memory cache-store.**  
  
### When to Use Interceptor Vs Cach Manager in Nestjs  
  
Let's just point out that we can use caching with two different approaches:  
  
1. The **Interceptor** approach  
   this approach does not allow us to delete from cache or update certain elements manually.   
2. The **Cache Manager** approach with **dependency injection**  
  
The interceptor approach is cleaner, but the cache manager approach gives you more flexibility with some overhead.  
  
As a rule of thumb, you will use the <font style="color:olivedrab">Cache Interceptor</font> If you need an endpoint to <font style="color:indianred">return cached data from the primary database</font> in a traditional CRUD app.  
  
You will use the `Cache Manager` if you need more control, like:  
- Deleting from cache  
- Updating cache  
- Manually fetching data from the cache store  
- A combination of the above 👆🏻  
  
To give a practical <u>example</u>, if you need to get a list of posts and you have an endpoint that fetches that list from the database. You need to use a cache interceptor.  
  
→ 따라서 Interceptor보다 Cache Manager를 사용하는 것이 토큰 관리에 더 쉬울 것이라는 판단이 들었다.  
  
  
  
Let's just point out that we can use caching with two different approaches:  
  
- **The Interceptor approach**  
- **The Cache Manager approach with dependency injection**  
  
  
  
  
### Cash Manager  
```shell  
npm install cache-manager  
npm install -D @types/cache-manager  
```  
  
  
```shell  
npm i cache-manager-redis-store redis  
npm i -D @types/cache-manager-redis-store  
```  
This small library uses the node-redis (unfortunately, it does not support ioredis)   
  
- 오류 해결: [ CacheModule Config 오류](../%EB%B6%81%EB%A7%88%ED%81%AC/%EC%97%90%EB%9F%AC%20%ED%95%B4%EA%B2%B0.md#^2d5ab6) 정리  
  
# Redis 메서드  
[출처](https://velog.io/@hahaha/NestJS-Redis를-이용한-Cache-구현) [명령어 출처](https://tre2man.tistory.com/277)  
```shell  
# redis 실행  
$ brew services start redis  
  
# redis server 실행  
$ redis-server  
  
# redis server 접속  
$ redis-cli  
# redis-cli -h localhost -p 6380  
  
# redis 서버 응답 테스트  
$ redis-cli ping  
// 'PONG' 으로 응답이 오면 서버 정상작동  
  
  
set [key] [value] : key에 해당하는 value 값을 입력한다.  
  
get [key] : key에 해당하는 value 값을 받아온다.  
  
keys * : 모든 캐시 데이터를 불러온다.  
  
ttl [key] : 해당 key의 삭제 되기까지의 시간을 구한다. (단위는 초)  
  
Fluslall : 모든 캐시 삭제  
  
  
```  
Redis Server 를 인터페이스로 직접 다루고싶으면 Docker Compose 를 사용하면 된다.  
```bash  
for key in $(redis-cli -p 6379 keys \*);  
  do echo "Key : '$key'"   
     redis-cli -p 6379 GET $key;  
done  
```  
  
[How to List Redis Keys | ObjectRocket](https://kb.objectrocket.com/redis/how-to-list-redis-keys-696)  
  
[API with NestJS #24. Cache with Redis. Running the app in a Node.js cluster](https://wanago.io/2021/01/11/nestjs-cache-redis-node-js-cluster/)  
  
## Managing Our Redis Servier with an Interface  
도커 대신 로컬 환경에서 쓰는 법 없나 검색해봤더니 [GitHub - luin/medis](https://github.com/luin/medis) 이런 프로그램을 발견했다. 유료임 ㅡㅡ  
일단 터미널 단에서 사용해보는 걸로 해보자.  
  
+ 더 찾아보니 이런게 있다. [GitHub - joeferner/redis-commander: Redis management tool written in node.js](https://github.com/joeferner/redis-commander)  
  
  
## Making Access & Refresh Tokens Revocable  
우리는 각 토큰에 고유 아이디를 붙여주었고 각기 추적할 수 있다.  
  
Once a client logs in successfully, the API stores the refresh token in a Redis bucket, indexed by its `jti`, together with metadata for the token's status.  
  
Since Redis stores data in `key-value` pairs, we can visualize the resulting data structure like so:  
  
- refresh-token-redis-bucket.json  
```json  
    {  
        "443d8502-eddb-4138-bee2-7a46cb5541d5": {  
            "isActive": true  
         },  
        "1811bcd1-f7b0-40aa-9517-85dbf89d43d0": {  
            "isActive": false // this refresh token has been disabled.  
        },  
        "d6383aaf-e465-4957-aa62-2e57bacf4327": {  
            "isActive": true  
        }  
​  
    }  
```  
  
Access tokens are indexed by their jti and stored in a different Redis bucket. However, the data structure looks the same:  
- access-token-redis-bucket.json  
```json  
    {  
        "86dc2e6d-d964-4022-915b-9bd2ec01df14": {  
            "isActive": true  
         },  
        "611b648a-d2e7-4204-9f3b-a92360dafa0e": {  
            "isActive": false // this access token has been disabled.  
        },  
        "7095d046-20a6-4ebf-9273-72c13bc98e59": {  
            "isActive": true  
        }  
    }  
```  
  
access token을 쓸때 우리는 페이로드에서 refreshTokenId 를 통해서 해당 refreshToken을 찾을 수 있다.  
(리프레시 토큰을 통해서 accessToken을 찾는 방법도 구현할 수는 있겠으나 굳이? 필요 없음)  
  
Lastly, we need a bucket that will help us keep track of which accounts generated refresh tokens belong to. This will simply contain a list of tokens.  
  
- users-redis-bucket.json  
```json  
{  
    "tokens": [  
      "443d8502-eddb-4138-bee2-7a46cb5541d5"  
    ]  
}  
```  
  
  
? redis cache 가 아니라 redis database 써야하는건가? namespace 별로 어떻게 K:V 분리할 수 있지?  
[Redis “Namespace” - Norman Fung - Medium](https://norman-lm-fung.medium.com/redis-namespace-2913a9708301) 를 참고하니까  
```shell  
127.0.0.1:6379> set 'ns1:key1' 'val1'  
OK  
127.0.0.1:6379> set 'ns1:key2' 'val2'  
OK  
127.0.0.1:6379> get 'ns1:key1'  
"val1"  
127.0.0.1:6379> get 'ns1:key2'  
"val2"  
127.0.0.1:6379> set 'ns1:l1:key1' 'val1_ns1_l1'  
OK  
127.0.0.1:6379> set 'ns1:l1:key2' 'val2_ns1_l1'  
OK  
```  
이렇게 쓰면 된다고 한다.  
![Pasted image 20230111191100.png](../../attachments/Pasted%20image%2020230111191100.png)   
  
From Redis RDB (Free on Linux), “namespaces” are represented by folders.  
  
  
[nestjs-redis/redis.md at main · liaoliaots/nestjs-redis · GitHub](https://github.com/liaoliaots/nestjs-redis/blob/main/docs/latest/redis.md)  
  
  
# 에러 오류 HTTPSTATUS: 401, 403  
  
당연히 알아야하는거? →   
  
로그인이 60 - 70퍼