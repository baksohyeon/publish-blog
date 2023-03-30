---  
category: 질문 목록/Solved Problems/Concept  
share: true  
title: Jest 테스트코드 Describe vs It  
tags:  
  - jest  
---  
  
  
# Jest 테스트코드 Describe Vs It  
  
**제목**: What is the difference between describe and it in Jest?  
**질문 날짜**: 2022-09-29  
**태그**: #jest  
**관련 글**: [Making Test Code at Controller layer](../../../Project/Testing/Making%20Test%20Code%20at%20Controller%20layer.md) [Making GetByAuther Test Code (Moking)](../../../Project/Testing/Making%20GetByAuther%20Test%20Code%20(Moking).md)[Nest js Create 테스트코드](../../../Project/Testing/Nest%20js%20Create%20%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%BD%94%EB%93%9C.md)  
  
---  
  
## 질문 내용  
  
https://stackoverflow.com/questions/32055287/what-is-the-difference-between-describe-and-it-in-jest  
  
## 질문 답변 (해결 방안)  
  
**`describe`** breaks your test suite into components. Depending on your test strategy, you might have a describe for each function in your class, each module of your plugin, or each user-facing piece of functionality.  
  
You can also nest describes to further subdivide the suite.  
  
**`it`** is where you perform individual tests. You should be able to describe each test like a little sentence, such as "it calculates the area when the radius is set". You shouldn't be able to subdivide tests further-- if you feel like you need to, use describe instead.  
  
```ts  
describe('Circle class', function() {  
  describe('area is calculated when', function() {  
    it('sets the radius', function() { ... });  
    it('sets the diameter', function() { ... });  
    it('sets the circumference', function() { ... });  
  });  
});  
```  
  
As the jest docs says, test and it are the same: https://jestjs.io/docs/en/api#testname-fn-timeout  
  
> test(name, fn, timeout)  
> Also under the alias: it(name, fn, timeout)  
  
and describe is just for when you prefer your tests to be organized into groups: https://jestjs.io/docs/en/api#describename-fn  
  
> describe(name, fn)  
  
`describe(name, fn)` creates a block that groups together several related tests.  
For example, if you have a `myBeverage` object that is supposed to be delicious but not sour, you could test it with:  
  
```ts  
const myBeverage = {  
  delicious: true,  
  sour: false,  
};  
  
describe("my beverage", () => {  
  test("is delicious", () => {  
    expect(myBeverage.delicious).toBeTruthy();  
  });  
  
  test("is not sour", () => {  
    expect(myBeverage.sour).toBeFalsy();  
  });  
});  
```  
  
This isn't required - you can write the test blocks directly at the top level. But this can be handy if you prefer your tests to be organized into groups.  
  
---  
