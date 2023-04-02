---
category: 질문 목록/Solved Problems/Concept
share: true
title: spyOn()과 jest.fn()의 차이
tags: [jest, NestJs]
---

# spyOn()과 jest.fn()의 차이

**관련 글**: [Nest js Create 테스트코드](../../../Project/Testing/Nest%20js%20Create%20%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%BD%94%EB%93%9C.md)

---

## 질문 내용

두 메서드의 차이가 뭔지 잘 모르겠음.
spyOn()이 더 발전된 메서드같은데.. (모킹보다 더 추가적인 기능이 있는 것 아님?)
굳이 모킹을 쓰는 이유가 뭐지?

## 질문 관련 서치 내용

![스크린샷 2022-09-28 오후 6.19.59.png](../../../Attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-28%20%EC%98%A4%ED%9B%84%206.19.59.png)
검색해보면

> The key thing to remember about `jest.spyOn` is that it is just ==sugar for the basic `jest.fn()` usage==. We can achieve the same goal by storing the original implementation, setting the mock implementation to to original, and re-assigning the original later:

라고 하고

jest 내부 코드 보면
[jest/index.js at e9aa321e0587d0990bd2b5ca5065e84a1aecb2fa · facebook/jest · GitHub](https://github.com/facebook/jest/blob/e9aa321e0587d0990bd2b5ca5065e84a1aecb2fa/packages/jest-mock/src/index.js#L674-L708)

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

이렇게 mockImplementation이 내부적으로 구현되어있는 것 아님? (코드 이해 못함)

[reactjs - What is the difference between jest.fn() and jest.spyOn() methods in jest? - Stack Overflow](https://stackoverflow.com/questions/57643808/what-is-the-difference-between-jest-fn-and-jest-spyon-methods-in-jest)질문 보면
**`jest.fn()`**

- You want to mock a function and really don't care about the original implementation of that function (it will be overridden by` jest.fn()`)
- Often you just mock the return value
- This is very helpful if you want to remove dependencies to the backend (e.g. when calling backend API) or third party libraries in your tests
- It is also extremly helpful if you want to make real unit tests. You don't care about if certain function that gets called by the unit you test is working properly, because thats not part of it's responsibility.

**`jest.spyOn()`**

- The original implementation of the function is relevant for your test, but:
  - You want to add your own implementation just for a specific scenario and then reset it again via `mockRestore()` (if you just use a `jest.spyOn()` without mocking it further it will still call the original function by default)
- You just want to see if the function was called
- I think this is especially helpful for integration tests, but not only for them!
- it allow you to convert an existing method on an object into a spy, that also allows you to track calls and re-define the original method implementation.

라고 한다.

> To my understanding the only difference is that YOU CAN RESTORE ORIGINAL FUNCTION with jest.spyOn and you can't with jest.fn.

### Spies Vs Stubs and Mocks

```js
// jest.fn()
test("app() with mock counter .toHaveBeenCalledTimes(1)", () => {
  const mockCounter = {
    increment: jest.fn(),
  };
  app(mockCounter);
  expect(mockCounter.increment).toHaveBeenCalledTimes(1);
});

// spyOn()
test("app() with jest.spyOn(counter) .toHaveBeenCalledTimes(1)", () => {
  const incrementSpy = jest.spyOn(counter, "increment");
  app(counter);
  expect(incrementSpy).toHaveBeenCalledTimes(1);
});
```

The main difference is that the `mockCounter` version wouldn’t allow the counter to increment.

So for example with the `spyOn(counter)` approach, we can assert that `counter.increment` is called but also `getCount()` and assert on that.

## 질문을 정리하면서 새롭게 얻은 해답이 있는가?

값만 반환하는 것을 보고 싶을 때 간편한게 mock이라서인건가?
모킹없이 스파이만 부르면 오리지널에 도청기 붙인거고 추가적으로 모킹 기능을 할 수 있다는건데
다 할 수 있는 spyOn() 안쓰고 왜 모킹을 쓰지?

spyOn() 은 원래 함수도 복원할 수 있음 → `mockRestore()` 메서드 쓰면 됨

`해답`: if you want to make an existing implementation a spy use ==spyOn== if you are building a mock, use ==fn()==.

## 추가 질문

원래 함수를 쓸 일이 있으면 spyOn쓰고 마네킹이 필요하면 fn() 쓰면 되는 건가여

언제 spyOn()
Mocking
구체적으로 언제 쓰는지

## 질문 답변 (해결 방안)

> 내가 찾아본게 맞음
