---
category: Project/basic
share: true
---
  
[Nest.js Step-by-Step](https://www.codemag.com/Article/1907081/Nest.js-Step-by-Step)  
# 개념  
[NestJS Interceptor와 Lifecycle](https://www.superb-ai.com/ko-blog/nestjs-interceptor-and-lifecycle)  
## Request Lifecycle  
  
1. Request가 들어옵니다.  
  
2. Middleware를 타게 됩니다. Middleware는 앞서 말씀드린 것과 같이 request와 response 중간에 로직을 추가합니다.  
  
3. Guard를 지나게 됩니다. Guard에선 authorization과 authentication을 해주게 됩니다. Guard는 허용된 유저가 아니면 요청 자체를 막아줍니다. 예를 들어 모든 사용자가 민감한 정보 혹은, 본인의 계정이 아닌 다른 계정에 접근할 수 있다면 보안상 굉장히 위험할 텐데요.  웹사이트의 관리자 권한을 아무나 사용 가능하다면 해당 웹 사이트는 많은 문제가 생길 것입니다. 이런 불상사를 막기 위해서 HTTP Header에 User의 정보가 담긴 Token을 보내면, 서버의 Guard가 유효한 유저인지 권한이 있는 유저인지 체크합니다.  
  
4. Pre Interceptor를 거치게 됩니다. Interceptor는 request와 response 중간에 로직을 추가합니다.  
  
5. Pipe를 거치게 됩니다. Pipe는 request가 왔을 때 body나 params, query에 대해 validation, transformation을 해줍니다. Pipe에는 두 가지 일반적인 사용 사례가 있는데요. 첫 번째로 transformation은 입력데이터를 원하는 출력으로 변환시킵니다. 두 번째로 validation은 입력 데이터를 검증합니다. 만약 오류가 있다면 에러가 발생됩니다.  
  
6. Controller를 과정을 지납니다. Controller는 앱에 대한 특정 request를 수신해 라우팅을 해줍니다. 일반적으론 service에 라우팅을 해주는 역할로 쓰입니다.  
  
7. Service layer가 있다면 service layer가 실행됩니다.  
  
8. Post Interceptor를 거칩니다.  
  
9. Exception filter를 거치고 여기서 400, 500번대 에러에 대한 에러 처리를 합니다. 보통 global exception filter를 선언해 에러에 대한 핸들링을 합니다. 이후 server response가 나가게 되며 Lifecycle이 종료됩니다.  
  
![Pasted image 20230223182817.png](../../attachments/Pasted%20image%2020230223182817.png)  
  
  
![스크린샷 2022-10-19 오후 10.50.46.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-10-19%20%EC%98%A4%ED%9B%84%2010.50.46.png)  
![Pasted image 20221024215647.png](../../attachments/Pasted%20image%2020221024215647.png)  
  
https://docs.nestjs.com/faq/request-lifecycle  
  
  
  
<iframe width="560" height="315" src="https://www.youtube.com/embed/x1W3FJ1RJlM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  
  
  
  
![스크린샷 2022-10-19 오후 10.52.17.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-10-19%20%EC%98%A4%ED%9B%84%2010.52.17.png)  
  
  
  
**Flow 순서대로 감**  
# **Incoming request**  
# 미들웨어  
`<<interface>>`: NestMiddleware  
> - Can mutate request and response objects  
> - Support for existing Express middleware libraries   
> - Maybe a good candidate for handling auth 'authentication' layer  (token validation, attaching props to request object, etc.)  
  
- Globally bound middleware  
- Module bound middleware  
  
# 가드  
`<<interface>>`: CanActivate  
> - Determines whether a given request will be handled depending on certain conditions (ACL, roles, permissions, etc)  
> - Essentially just conform to this canActivate interface and it just tells if we can continue on to the route handler based on sort of authorization  
> - Similar to auth "authorization" layer  
  
- Global guards  
- Controller guards  
- Route guards  
![스크린샷 2022-10-19 오후 11.04.34.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-10-19%20%EC%98%A4%ED%9B%84%2011.04.34.png)  
# 인터셉터  
`<<interface>>`: NestInterceptor  
> 1. Transform incoming data  
> 2. Transform leaving API data  
> 3. Stream overriding (e.g caching incoming requests)  
> 인터셉터는 요청과 응답을 가로채서 변형을 가할 수 있는 컴포넌트입니다. (인터셉터로 요청과 응답을 입맛에 맞게 바꾼다: )  
> Powerful form of the request-response pipeline and that's because we have direct access to the request `before route handler` and we can also mutate the response `after route handler`  
>  
  
In the interceptor we can actually call `next.handle` and `pipe` nest js operators on the response.  
So we can do things like mutating the response into a form that we prefer we can do things like logging how long the request took before and after the route handler we can do some global error handling.  
There's really an endless amount of possibilities because we have access to the request before and the response after the round handler.  
So in between this interceptor here before the route handler this is where pipes come into play and pipes are pretty straightforward they essentially just take in the request object from the route handler and it allows us to transform input data.  
  
So we can essentially complettely transform data that's going to the route handler into any form we'd like and we can also validate it.   
So if it doesnt conform to a certain standard like `json` schema validation we can throw an error and prevent it from reaching the route handler.   
  
인터셉터는 AOP(관점 지향 프로그래밍)에 영향을 많이 받았습니다. 인터셉터를 이용하면 다음과 같은 기능을 수행할 수 있습니다.  
  
-   메서드 실행 전/후 추가 로직 바인딩  
-   함수에서 반환된 결과를 변환  
-   함수에서 던져진 예외를 변환  
-   기본 기능의 동작을 확장  
-   특정 조건에 따라 기능을 완전히 재정의(예: 캐싱)  
  
[[nestjs] ClassSerializerInterceptor 전체 적용하기](https://trend21c.tistory.com/2259)  
  
  
```ts  
intercept(context: ExecutionContext, next: CallHandler) {  
  context.getClass<T>(): Type<T>  
  context.getHandler(): Function  
  next.handle(): Observable  
}  
```  
  
- Global interceptors (pre-controller)  
- Controller interceptors (pre-controller)  
- Route interceptors (pre-controller)  
  
참고 문서:  
[13.1 인터셉터(Interceptor) - NestJS로 배우는 백엔드 프로그래밍](https://wikidocs.net/158655)  
[Interceptors | NestJS - A progressive Node.js framework](https://docs.nestjs.com/interceptors)  
[Nestjs 프레임워크 서버(인터셉터) -6](https://lts0606.tistory.com/621?category=955340)  
# 파이프  
`<<interface>>`: PipeTransform  
> - Transform input data going into route handler, *i.e* operates on arguments destined for the controller method  
> - Vailidate input data  
> - Strip certain data properties  
> - Built-in pipes: ValidationPipe, ParseIntPipe, ParseUUIDPipe  
  
- Global pipes  
- Controller pipes  
- Route pipes  
- Route parameter pipes  
![스크린샷 2022-10-19 오후 11.04.34.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-10-19%20%EC%98%A4%ED%9B%84%2011.04.34.png)  
  
# 컨트롤러  
Controller (method handler)  
# 서비스  
Service (if exists)  
  
# Exception Filters  
`<<interface>>`: ExceptionFilter  
> the last part of our request response pipeline is exception filters.  
> Exception filters are essentially a catch block you can think around our whole request response-pipeline that if an http exception  or any exception is thrown.  
> We can route that request to our exception filter and transform into a standard error response.  
  
- Route interceptor (post-request)  
- Controller interceptor (post-request)  
- Global interceptor (post-request)  
- Exception filters (route, then controller, then global)  
# **Server response**  
  
> It should be noted we can directly inject any nest js @Injectable() dependencies into any stage of this request-response pipeline.  
  
  
  
---  
  
  
  
  
## Strategy 패턴  
> 내일 정리하기  
https://refactoring.guru/design-patterns/strategy/typescript/example  
Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.  
  
  
  
  
  
## 에러핸들링  
https://docs.nestjs.com/exception-filters#exception-filters  
### Built-in HTTP Exceptions  
- `BadRequestException`  
- `UnauthorizedException`  
- `NotFoundException`  
- `ForbiddenException`  
- `NotAcceptableException`  
- `RequestTimeoutException`  
- `ConflictException`  
- `GoneException`  
- `HttpVersionNotSupportedException`  
- `PayloadTooLargeException`  
- `UnsupportedMediaTypeException`  
- `UnprocessableEntityException`  
- `InternalServerErrorException`  
- `NotImplementedException`  
- `ImATeapotException`  
- `MethodNotAllowedException`  
- `BadGatewayException`  
- `ServiceUnavailableException`-   
- `GatewayTimeoutException`  
- `PreconditionFailedException`  
  
  
## Exceptions & Handling Exceptions  
> How to set up a get request with a route parameter   
> We can actually just return an actual object itself directly back to the user and if the object was not found we just throw an error and nest will handle that for us underneath the hood and it will return the appropriate response okay so return a 403 if we specify.  
  
  
추가 참고 자료  
[nest.js](https://roseline.oopy.io/fa3fe94a-fac8-4ae7-9cab-597256ca88cb)  
  
---   
## JWT & 가드 공부  
[jwt 원리](http://www.opennaru.com/opennaru-blog/jwt-json-web-token/#:~:text=JWT%EB%8A%94%20%EC%84%9C%EB%B2%84%EC%99%80%20%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8,%EC%88%98%20%EC%9E%88%EB%8A%94%20%EB%AC%B8%EC%9E%90%EB%A7%8C%EC%9C%BC%EB%A1%9C%20%EB%A7%8C%EB%93%AD%EB%8B%88%EB%8B%A4.)  
https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-JWTjson-web-token-%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC  
https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-Access-Token-Refresh-Token-%EC%9B%90%EB%A6%AC-feat-JWT  
  
  
