---  
category: 잡동사니 🧸  
share: true  
date created: Monday, January 16th 2023, 3:20:49 pm  
date modified: Wednesday, January 18th 2023, 4:41:28 pm  
---  
# HTTP 프로토콜  
참고자료 https://moon-seung-chan.tistory.com/5  
https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview  
[참고 자료](https://novlog.tistory.com/245#:~:text=HTTP%EB%8A%94%20%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9C%BC%EB%A1%9C%20Request,Response(%EC%9D%91%EB%8B%B5)%EC%9D%84%20%EB%B3%B4%EB%82%B8%EB%8B%A4.)  
  
![[../assets/img/스크린샷 2022-10-06 오후 3.15.21.png|스크린샷 2022-10-06 오후 3.15.21.png]]  
  
`HTTP`: HTML, JS, CSS 파일 웹서버에게 요청하고 받아오는 프로토콜  
`HTML + CSS + JavaScript`  
  
  
  
  
  
# 리소스  
1 .미디어 타입: `MIME`  
HTTP는 수천 개의 데이터 타입을 다루기 때문에 ‘MIME (Multipurpose Internet Mail Extensions, 다목적 인터넷 메일확장)‘  
라는 데이터 포맷 라벨을 웹에서 전송되는 객체 각각에 붙인다. 쉽게 말하면, 웹 콘텐츠가 어떤 데이터 타입인지 알려주는 라벨    
  
표현 형식: ‘주 타입/부 타입’  
HTML 데이터 타입 = ‘text/html’  
JPEG 이미지 데이터 타입 = ‘image/jpeg’  
GIF 이미지 데이터 타입 = ‘image/gif’  
  
  
2. URI, URL, URN  
https://www.geeksforgeeks.org/components-of-a-url/  
![[../assets/img/Pasted image 20221008183144.png|Pasted image 20221008183144.png]]  
  
URI: 통합 자원 식별자 (Uniform Resource Identifier)  
웹 서버 리소스의 이름 (인터넷의 우편물 주소 같은 역할)  
  
URL: 통합 자원 지시자 (Uniform Resource Locator)  
특정 서버의 한 리소스에 대한 구체적 위치 서술  
  
URN: 유니폼 리소스 이름 (Uniform Resource Name)  
리소스의 위치에 영향을 받지 않는 유일무이한 이름을 서술  
  
오늘날 대부분의 URI는 URL을 뜻한다.  
  
  
  
![[../assets/img/Pasted image 20221008175927.png|Pasted image 20221008175927.png]]  
  
HTTP는 기본적으로 Response/Request 구조로 구성되어 있다.  
# 통신 과정  
1. 웹브라우저는 서버 url에서 호스트명을 추출한다  
2. 웹브라우저는 서버 호스트명을 ip로 변환한다 (DNS)  
3. (만약 포트번호가 있다면) url에서 포트번호를 추출한다  
4. 웹브라우저는 웹서버와 TCP 커넥션을 맺는다  
5. 웹브라우저는 서버에 HTTP 요청을 보낸다  
6. 서버는 웹브라우저에 HTTP 응답을 돌려준다  
7. 커넥션이 닫히면, 웹브라우저는 문서를 보여준다.  
  
  
# 메시지  
![[../assets/img/Pasted image 20221008180132.png|Pasted image 20221008180132.png]]  
  
1.**시작줄**  (Start line = `요청` <u>request line</u> + `응답` <u>status line</u> )  
 요청 및 응답에 대한 내용    
[Request Line 구조]   
1. HTTP method: 서버가 수행해야 할 동작 나타냄 ex. GET, POST, DELETE, PUSH 등..  
2. Request-target: 주로 URL, PORT, 도메인 등이 붙는 절대경로 (`/` 로 시작하는 path)  
3. HTTP version: Requset Message에서 사용해야 할 HTTP 버전을 나타내어 준다.  
  
[Status Line 구조]  
1. HTTP version: HTTP 버전  
2. Status code: 요청의 성공 혹은 실패를 나타낸다 ex. 200, 404, 500  
3. Reason-phrase: 사람이 이해할 수 있는 간단한 문구를 적어줌  
  
2.**헤더**    
 HTTP 전송에 필요한 모든 부가정보가 들어있다.  (Content-Length, Content-Type)  
 쌍점(:)으로 구분되어 있는 하나의 이름과 하나의 값으로 구성  (field - value)  
 0개 이상의 헤더 필드로 구성 (field 이름은 대소문자 구분 X)  
 헤더는 빈 줄로 끝남    
 [ 요청 헤더 ]   
 ![[../assets/img/스크린샷 2022-10-08 오후 6.50.04.png|스크린샷 2022-10-08 오후 6.50.04.png]]  
 ![[../assets/img/스크린샷 2022-10-08 오후 6.50.35.png|스크린샷 2022-10-08 오후 6.50.35.png]]  
 `CR + LF` : 공백라인  
  
3.**본문**    
 어떤 종류의 데이터든 들어갈 수 있는 메시지 본문  
 요청: 웹 서버로 데이터를 실어보냄  
 응답: 클라이언트로 데이터 반환  
 임의의 이진 데이터 포함 가능 (이미지, 비디오, 오디오, 텍스트 등)  
  
1) 메서드  
서버에게 어떤 동작이 취해져야 하는지 알려주는 역할  
모든 HTTP 요청 메시지는 한 개의 메서드를 가짐  
GET : 클라이언트가 서버의 리소스를 요청한다.  
POST : 클라이언트가 데이터를 서버 게이트웨이 애플리케이션으로 보낸다.  
PUT : 클라이언트가 서버의 리소스를 갱신/생성 한다.  
DELETE : 클라리언트가 서버의 리소스를 삭제한다.  
  
2) 상태코드  
200 : 성공. 문서가 바르게 반환  
300 : 리소스 위치 변경  
400 : 리소스를 찾을 수 없음  
500 : 서버내의 에러  
  
오서오세요 제 포클레인에  
  
# Idempotent  
https://www.mscharhag.com/p/rest-api-design  
![[../assets/img/스크린샷 2022-10-08 오후 7.15.14.png|스크린샷 2022-10-08 오후 7.15.14.png]]  
`Idempotency` means that multiple identical requests will have the same outcome.   
So it does not matter if a request is sent once or multiple times. The following HTTP methods are idempotent: GET, HEAD, OPTIONS, TRACE, PUT and DELETE.   
All safe HTTP methods are idempotent but PUT and DELETE are idempotent but not safe.  
safe: 리소스 변경이 없는 것  
= 여러번 시도해도 같은 결과가 나오는 것   
  
  
  
Idempotency is a positive feature of an API because it can make an API more fault-tolerant.   
Assume there is an issue on the client and requests are send multiple times.   
As long as idempotent operations are used this will cause no problems on the server side.  
  
  
# REST / HTTP methods: POST vs. PUT vs. PATCH  
https://www.mscharhag.com/api-design/updating-resources-put  
https://www.mscharhag.com/api-design/http-post-put-patch  
https://irostub.github.io/web/idempotent/ >> 보고 읽기  
  
  
  
  
  
  
# HTTP 프로토콜에 대해 찾아보면 좋을 사이트 (HTTP 완벽가이드 책 출처)  
1. HTTP Pocket Reference  
클린턴 윙(Clinton Wong), 오라일리 출판사(O'Reilly & Associates, Inc). 이 작은 책은 HTTP에 대해 간략하게 소개하며, HTTP 트랜잭션을 구성하는 각 헤더와 상태코드에 대한 설명을 찾아보기 쉬운 형태로 제공한다.  
  
2. http://www.w3.org/Protocols/  
이 W3C 웹페이지는 HTTP 프로토콜에 대한 여러 훌륭한 링크를 포함한다.  
  
3. http://www.ietf.org/rfc/rfc2616.txt  
RFC 2616, "Hypertext Transfer Protocol-HTTP/1.1"은 HTTP/1.1의 현재 버전에 대한 공식 명세다. 이 명세는 HTTP에 대한 잘 쓰였고 잘 조직된 상세한 참고문서이지만, HTTP의 근간이 되는 개념 및 계기나 이론과 실제의 차이를 배우길 원하는 독자에게는 이상적이지 않다. 우리는 이 책이 그러한 개념을 채워주어 독자가 이 명세를 더 잘 활용하게 되기를 희망한다.  
  
4. http://www.ietf.org/rfc/rfc1945.txt  
RFC 1945, "Hypertext Transfer Protocol-HTTP/1.0"은 오늘날 HTTP의 초석이 된 HTTP/1.0을 서술한 정보성(informational) RFC이다. 이 명세가 쓰일 당시를 기준으로, 공식적으로 허용되는 동시에 모범이 될 만한 웹 애플리케이션의 행동방식들에 대해 자세히 설명한다. 또한 HTTP/1.1에서는 더 이상 사용하지 않게 되었지만 여전히 구식 애플리케이션에서는 널리 구현되어 있는 동작들에 대한 유용한 설명도 포함한다.  
  
5. http://www.w3.org/Protocols/HTTP/Aslmplemented.html  
이 웹페이지에는 GET 요청만이 구현되어 있고 내용 유형 개념도 없는 1991년의 HTTP/0.9프로토콜에 대한 설명이 담겨있다.  
  
  
  
## 역사  
1. http://www.w3.org/Protocols/WhyHTTP.html  
1991년에 HTTP의 저자에 의해 만들어진 이 간략한 웹페이지는, HTTP의 최소주의적인 본래 목표 중의 일부를 강조한다.  
  
2. http://www.w3.org/History.html  
"A Little History of the World Wide Web"은 월드 와이드 웹과 HTTP의 초기 목표와 탄생에 대한 짧지만 재미있는 시각을 제공한다.  
  
3. http://www.w3.org/Designlssues/Architexture.html  
"Web Architecture from 50,000 Feet"는 HTTP를 비롯한 관련 웹 기술에영향을 미친 설계 원리와 월드 와이드 웹에 대해 폭넓고도 대담한 시각으로 서술한다.  
  
  
  
## 기타 월드 와이드 웹 정보  
1. http://www.w3.org  
월드 와이드 웹 컨소시엄(W3C)은 웹을 기술적으로 주도하는 팀이다. W3C는 진화하는 웹을 위한 상호 교환이 가능한 기술(명세, 가이드라인, 소프트웨어, 도구)을 개발한다. W3C사이트는 웹 기술에 대한 자세한 문서와 입문 자료의 보고다.  
  
2. http://www.ieft.org/rfc/rfc2396.txt  
RFC2396, "Uniform Resource Idenrifiers (URI): Generic Syntax"는 URI와 URL에 대한 자세한 참고서다.  
  
3. http://www.ietf.org/rfc/rfc2141.txt  
RFC 2141, "URN Syntax"는 URN 문법을 묘사한 1997년의 명세다.  
  
4. http://www.ietf.org/rfc/rfc2046/txt  
RFC 2046, "MIME part 2: Media Types"은 멀티미디어 콘텐츠 관리를 위한 다목적 인터넷 메일 확장 표준의 다섯 인터넷 명세 중 두 번째 것이다.  
  
5. http://www.wrec.org/Drafts/draft-ietf-wrec-taxonomy-06.txt  
인터넷 초안 "Internet Web Replication and Caching Taxonomy"는 웹 구성요소에 대한 표준 용어집이다.  
- [HTTP 역사 정리](https://yozm.wishket.com/magazine/detail/1686/)  
  
