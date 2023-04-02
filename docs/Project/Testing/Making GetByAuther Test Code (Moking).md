---  
category: Project/Testing  
share: true  
---  
# Describe it Jest 구글링  
  
- Getting started with Jest  
https://www.valentinog.com/blog/jest/#:~:text=Jest%20is%20a%20JavaScript%20test,default%20choice%20for%20React%20projects.  
  
## What Testing Means?  
In tech jargon testing means checking that our code meets some expectations. For example: a function called "transformer" should returns the expected output given some input.  
  
There are many types of testing and soon you'll be overwhelmed by the terminology, but long story short tests fall into three main categories:  
  
1. unit testing  
2. integration testing  
3. UI testing  
In this Jest tutorial we'll cover only unit testing, but at the end of the article you'll find resources for the other types of tests.  
  
## Jest Tutorial: what is Jest?  
Jest is a JavaScript test runner, that is, a JavaScript library for creating, running, and structuring tests.  
  
Jest ships as an NPM package, you can install it in any JavaScript project. Jest is one of the most popular test runner these days, and the default choice for React projects.  
  
  
## First Things First: how Do I Know what to Test?  
  
When it comes to testing, even a simple block of code could paralyze beginners. The most common question is "How do I know what to test?".  
  
If you're writing a web application a good starting point would be testing every page of the app and every user interaction. But, web applications are also made of units of code like functions and modules that need to be tested too.  
  
There are two scenarios most of the times:  
  
you inherit legacy code which comes without tests  
you have to implement a new functionality out of thin air  
What to do? For both cases you can help yourself by thinking of tests as of bits of code that check if a given function produces the expected result. Here's how a typical test flow looks like:  
  
import the function to test  
give an input to the function  
define what to expect as the output  
check if the function produces the expected output  
Really, that's it. Testing won't be scary anymore if you think in these terms: input - expected output - assert the result.  
  
Now hands on Jest!  
  
  
  
# Specifications and Test-driven Development  
As developers, we all like creativity freedom. But, when it comes to serious stuff most of the time you don't have so much privilege.  
  
We've got to follow specifications, that is, a written or verbal description of what to build.  
  
In this tutorial we've got a rather simple spec from our project manager. A super important client needs a JavaScript function that should filter an array of objects.  
  
For every object we must check a property called "url" and if the value of the property matches a given term then we should include the matching object in the resulting array.  
  
Being a test-savvy JavaScript developer you want to follow test-driven development, a discipline which imposes to write a failing test before starting to code.  
  
```ts  
describe("Filter function", () => {  
  test("it should filter by a search term (link)", () => {  
    const input = [  
      { id: 1, url: "https://www.url1.dev" },  
      { id: 2, url: "https://www.url2.dev" },  
      { id: 3, url: "https://www.link3.dev" }  
    ];  
  
    const output = [{ id: 3, url: "https://www.link3.dev" }];  
  
    expect(filterByTerm(input, "link")).toEqual(output);  
  
  });  
});  
  
```  
  
# GetByAuthor 테스트 코드 짜기  
  
```ts  
// blog.controlles.ts   
// 테스트하고 싶은 로직  
  
  @Get('/author/:author')  
  async getBoardByAuthor(  
    @Param('author') author: string,  
  ): Promise<GetBoardResponseDto[]> {  
    return this.blogService.getBoardsByAuthor(author);  
  }  
  
```  
  
- mockBlogService 비즈니스 로직 모킹  
```ts  
  
    getBoardsByAuthor: jest.fn().mockImplementation((author: string) =>  
      Promise.resolve({  
        title: '1',  
        description: '1',  
        body: '1',  
        author: '1',  
      }),  
    ),  
  });  
```  
  
- 테스트 로직  
```Ts  
describe('getBoardsByAuthor', () => {  
// test 1  
    it('should be return one Board', async () => {  
      await expect(  
        blogController.getBoardByAuthor('test author'),  
      ).resolves.toEqual({  
        title: '1',  
        description: '1',  
        body: '1',  
        author: '1',  
      });  
    });  
  
// test 2: jest.spyOn() 사용  
  });  
```  
  
  
  
---  
# 개념  
## Jest 사용법: 비동기 코드 (Async code)  
`참고`: https://velog.io/@modolee/jest-user-guide-05  
https://jestjs.io/docs/tutorial-async#asyncawait  
  
## spyOn()  
https://codewithhugo.com/jest-fn-spyon-stub-mock/  
사용하는 이유:   
- was the stub/spy called?  
- was the stub/spy called the right amount of times?  
- was the stub/spy called with the right arguments/parameters?  
  
Assertions for a spy/mock/stub beyond Jest  
### `jest.toBeCalled()`/`.toHaveBeenCalled()`: Assert a stub/spy Has Been Called  
  
  
### `.spyOn().mockImplementation()` To Replace a Spied-on function’s Implementation  
### `jest.not.toBeCalled()`/`.not.toHaveBeenCalled()`: Asserting a stub/spy Has not Been Called  
### `jest.toHaveBeenCalledTimes()`: Asserting on a stub/spy Call Count  
### `jest.toHaveBeenCalledWith()`: Asserting on parameter/arguments for call(s)  
  
### What is the Difference between jest.fn() and jest.spyOn() Methods in Jest?  
https://stackoverflow.com/questions/57643808/what-is-the-difference-between-jest-fn-and-jest-spyon-methods-in-jest  
  
## Understanding Jest Mocks  
https://medium.com/@rickhanlonii/understanding-jest-mocks-f0046c68e53c  
  
추가적으로 알게된 것만 필기..  
  
`jest.mock` does this automatically for all functions in a module  
`jest.spyOn` does the same thing but allows restoring the original function  
  
  
you may want to mock a function, but then restore the original implementation:  
  
```js  
// 코드 출처: Rick Hanlon II  
import * as app from "./app";  
import * as math from "./math";  
  
test("calls math.add", () => {  
  const addMock = jest.spyOn(math, "add");  
  
  // override the implementation  
  addMock.mockImplementation(() => "mock");  
  expect(app.doAdd(1, 2)).toEqual("mock");  
  
  // restore the original implementation  
  addMock.mockRestore();  
  expect(app.doAdd(1, 2)).toEqual(3);  
});  
```  
  
This is useful for tests within the same file, but unnecessary to do in an `afterAll` hook since each test file in Jest is sandboxed.  
  
  
> The key thing to remember about `jest.spyOn` is that it is just sugar for the basic `jest.fn()` usage. We can achieve the same goal by storing the original implementation, setting the mock implementation to to original, and re-assigning the original later:  
  
  
```js  
  
import * as app from "./app";  
import * as math from "./math";  
  
test("calls math.add", () => {  
  // store the original implementation  
  const originalAdd = math.add;  
  
  // mock add with the original implementation  
  math.add = jest.fn(originalAdd);  
  
  // spy the calls to add  
  expect(app.doAdd(1, 2)).toEqual(3);  
  expect(math.add).toHaveBeenCalledWith(1, 2);  
  
  // override the implementation  
  math.add.mockImplementation(() => "mock");  
  expect(app.doAdd(1, 2)).toEqual("mock");  
  expect(math.add).toHaveBeenCalledWith(1, 2);  
  
  // restore the original implementation  
  math.add = originalAdd;  
  expect(app.doAdd(1, 2)).toEqual(3);  
});  
```  
  
  
- 어떻게 spyOn()이 적용되는지 이해  
https://github.com/facebook/jest/blob/e9aa321e0587d0990bd2b5ca5065e84a1aecb2fa/packages/jest-mock/src/index.js#L674-L708 를 보면 알 수 있는데  
  
```js  
  spyOn(object: any, methodName: any, accessType?: string): any {  
    if (accessType) {  
      return this._spyOnProperty(object, methodName, accessType);  
    }  
  
    if (typeof object !== 'object' && typeof object !== 'function') {  
      throw new Error(  
        'Cannot spyOn on a primitive value; ' + this._typeOf(object) + ' given',  
      );  
    }  
  
    const original = object[methodName];  
  
    if (!this.isMockFunction(original)) {  
      if (typeof original !== 'function') {  
        throw new Error(  
          'Cannot spy the ' +  
            methodName +  
            ' property because it is not a function; ' +  
            this._typeOf(original) +  
            ' given instead',  
        );  
      }  
  
      object[methodName] = this._makeComponent({type: 'function'}, () => {  
        object[methodName] = original;  
      });  
  
      object[methodName].mockImplementation(function() {  
        return original.apply(this, arguments);  
      });  
    }  
  
    return object[methodName];  
  }  
```  
  
결국은 mock 함수를 갖다 쓰는거고 별반 차이 없는 것이라고 이해를 했음.  
  
# spyOn()과 jest.fn()의 차이  
[spyOn()과 jest.fn()의 차이](../../%EC%A7%88%EB%AC%B8%20%EB%AA%A9%EB%A1%9D/Solved%20Problems/Concept/spyOn()%EA%B3%BC%20jest.fn()%EC%9D%98%20%EC%B0%A8%EC%9D%B4.md#)  에서 정리해뒀다..  
  
  
  
