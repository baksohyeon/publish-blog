---
category: 질문 목록/Solved Problems/Error
share: true
title: 테스트 코드 짜는데 Dto 형식에 맞게 결과값(Date)을 반환할 때 오류
tags:
  - jest
  - NestJs
---
  
# 테스트 코드 짜는데 Dto 형식에 맞게 결과값(Date)을 반환할 때 오류  
  
## 제목: 테스트 코드 짜는데 결과값을 반환할 때 오류  
  
## 질문 날짜: 2022-09-28  
  
태그: #jest #NestJs    
**관련 글**: [Making GetByAuther Test Code (Moking)](../../../Project/Testing/Making%20GetByAuther%20Test%20Code%20(Moking).md)  
  
---  
  
## 질문 내용  
  
- **read-board.dto.ts**  
  ![스크린샷 2022-09-28 오후 5.32.48.png](../../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-28%20%EC%98%A4%ED%9B%84%205.32.48.png)  
  
- **blog.service.ts**  
  ![스크린샷 2022-09-28 오후 5.33.33.png](../../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-28%20%EC%98%A4%ED%9B%84%205.33.33.png)  
  
- **blog.controller.spec.ts**  
  ![스크린샷 2022-09-28 오후 5.38.25.png](../../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-28%20%EC%98%A4%ED%9B%84%205.38.25.png)  
  Dto대로 반환하려고 하는데 오브젝트 내에 Date 형식은 값이 못들어감...  
  
## 질문 관련 서치 내용  
  
`date dto nest js jest` 라고 검색  
NestJs - How to unit test a DTO?  
https://stackoverflow.com/questions/60852170/nestjs-how-to-unit-test-a-dto  
  
spec.dto 파일을 새롭게 만들어라함. 아닌듯  
  
`UNIT test dto and domain objects` 구글링함  
  
https://www.faqcode4u.com/faq/603987/what-is-the-best-way-to-mock-dtos-in-java  
  
> Generally DTO contain only fields and no logic which needs to be mocked out.  
> I would use a DTO as a mock of itself. If the DTO has logic in it you might like to mock out, I would move the logic out of the DTO.  
> To create DTO, I would do this from text, either in the test itself, or from an external file. You could use JSon, but if you don't use that already I would use XMLEncoder/XMLDecoder. Its not pretty XML but it is built in so you don't need an extra library.  
> If you can, you might be able to create DTOs from the logs of the application, so you can recreate a realistic scenario.  
  
## 질문을 정리하면서 새롭게 얻은 해답이 있는가?  
  
서칭해봐도 딱히 얻은 답이 없음  
  
## 해결:  
  
```ts  
const dateValue = new Date();  
```  
  
로 넣어줌  
  
## 질문이 나왔던 이유  
  
`new`로 선언할 수 있는건지 잘 몰랐음 ㅎ  
생각해보니 당연 ㅎ  
  
---  
