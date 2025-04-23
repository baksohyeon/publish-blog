---
category: 질문 목록/Solved Problems/Concept
share: true
title: spread 연산자 in Object
---
  
# Spread 연산자 in Object  
  
**관련 글**: [Nest js Create 테스트코드](../../../Project/Testing/Nest%20js%20Create%20%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%BD%94%EB%93%9C.md)  
  
---  
  
## 질문 내용  
  
https://github.dev/jmcdo29/testing-nestjs/tree/main/apps/typeorm-sample  
  
- catDTO  
  
```ts  
export interface CatDTO {  
  id?: string;  
  name?: string;  
  breed?: string;  
  age?: number;  
}  
```  
  
- mockingService  
  
```ts  
insertOne: jest  
  .fn()  
  .mockImplementation((cat: CatDTO) =>  
	Promise.resolve({ id: 'a uuid', ...cat }),  
  ),  
```  
  
여기서 예제코드 보는데  
  
```ts  
describe("newCat", () => {  
  it("should create a new cat", async () => {  
    const newCatDTO: CatDTO = {  
      name: "New Cat 1",  
      breed: "New Breed 1",  
      age: 4,  
    };  
    await expect(controller.newCat(newCatDTO)).resolves.toEqual({  
      id: "a uuid",  
      ...newCatDTO,  
    });  
  });  
});  
```  
  
이게 말이 되는 코드인가?  
  
## 질문 관련 서치 내용  
  
[Spread syntax (...) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)  
[나머지 매개변수와 스프레드 문법](https://ko.javascript.info/rest-parameters-spread#ref-261)  
  
## 질문 답변 (해결 방안)  
  
- `...`이 함수 매개변수의 끝에 있으면 인수 목록의 나머지를 배열로 모아주는 '나머지 매개변수’입니다.  
  [How to merge Objects in TypeScript | bobbyhadz](https://bobbyhadz.com/blog/typescript-merge-objects) 참고하기  
  
```typescript  
const obj1 = { name: "Tom", age: 30 };  
const obj2 = { name: "Alice" };  
  
// 👇️ const obj3: {name: string; age: number}  
const obj3 = { ...obj1, ...obj2 };  
console.log(obj3); // 👉️ {name: 'Alice', age: 30}  
```  
  
---  
