---  
category: Project/Testing  
share: true  
title: Test 관련 메모  
---  
[Jest 공식문서](https://jestjs.io/docs/api)  
  
# 단위 테스트 시나리오를 작성하자  
[Jest를 이용한 Unit Test 적용기. 처음 프론트엔드 테스트라는 것을 알게된 것은 3년전 Angular 1을… | by jinseok.choi | Medium](https://medium.com/@jinseok.choi/jest%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-unit-test-%EC%A0%81%EC%9A%A9%EA%B8%B0-420049c16cc8)  
  
  
```  
Story. 사용자는 Swiper를 생성할 수 있다.  
  senario  
    given: 설정없이  
    when: Swiper를 생성하면  
    then: default 속성을 가진 Swiper를 생성한다.  
  senario  
    given: width와 height을 설정하고  
    when: Swiper를 생성하면  
    then: 설정된 width와 height을 가진 Swiper를 생성한다.  
Story. 사용자는 다음 슬라이드로 이동할 수 있다.  
  senario  
    given: 다음 슬라이드가 있을 때  
    when: 다음 슬라이드를 요청하면,  
    then: 다음 슬라이드로 이동한다.  
  senario  
    given: 다음 슬라이드가 없을 때  
    when: 다음 슬라이드를 요청하면,  
    then: 현재 슬라이드를 유지한다.  
Story. 사용자는 이전 슬라이드로 이동할 수 있다.  
  senario  
    given: 이전 슬라이드가 있을 때  
    when: 이전 슬라이드를 요청하면,  
    then: 이전 슬라이드로 이동한다.  
  senario  
    given: 이전 슬라이드가 없을 때  
    when: 이전 슬라이드를 요청하면,  
    then: 현재 슬라이드를 유지한다.  
```  
  
# Integration Test  
[Integration testing with NestJS and TypeORM | by Paul Salmon | Medium](https://medium.com/@salmon.3e/integration-testing-with-nestjs-and-typeorm-2ac3f77e7628)  
# JavaScript Testing Tutorial  
[Types of JavaScript tests. Basics of unit testing with Jest - wanago.io](https://wanago.io/2018/08/27/testing-javascript-tutorial-types-of-tests-of-unit-testing-with-jest/)  
여기에 설명 겁나 잘되어있음  
  
With **Jest**, you can group them with the `describe` function. It creates a block that can combine several tests.  
  
> You may notice the usage of the it function instead of the **test** function. It is a commonly used alias. Running `it === test`  returns true.  
  
Grouping your tests like that make your code cleaner. You should care for the quality of both the code of your application and the code that tests it.  
  
**describe > it / test**  
  
  
NestJS에서 제공하는 <mark class="yellow">@nestjs/testing</mark> 패키지를 사용하면 테스트에 사용되는 종속성만 선언해서 모듈을 만들고 해당 모듈로 UserService, UserRepository를 가져올 수 있다.  
  
  
  
  
  
```js  
  
describe('in the math global object', () => {  
   
  describe('the random function', () => {  
    it('should return a number', () => {  
      expect(typeof Math.random()).toEqual('number');  
    })  
    it('should return a number between 0 and 1', () => {  
      const randomNumber = Math.random();  
      expect(randomNumber).toBeGreaterThanOrEqual(0);  
      expect(randomNumber).toBeLessThan(1);  
    })  
  });  
   
  describe('the round function', () => {  
    it('should return a rounded value of 4.5 being 5', () => {  
      expect(Math.round(4.5)).toBe(5);  
    })  
  });  
   
})  
```  
  
# Google Oauth 로그인 테스트 코드 짜기  
  
## AuthService 테스트  
### Creating Testing Modules  
```ts  
  
```  
  
  
# Mocking Typeorm  
mockRepository로 만든, fake repo들을 다른 typeorm의 entity들이 공유해서는 안되기 때문에 다음과 같이 함수 형태로 작성하는 것이 좋다[.](https://darrengwon.tistory.com/999)  
```ts  
const mockRepository = () => ({  
  findOne: jest.fn(),  
  save: jest.fn(),  
  create: jest.fn(),  
});  
```  
  
  
 mockJwtService는 다른 곳에서 중복되어 사용되지 않으니 함수형태로 만들지 않아도 됨.  
  
  
- typeorm test 참고 코드  
[testing-nestjs/apps/typeorm-sample at main · baksohyeon/testing-nestjs · GitHub](https://github.com/baksohyeon/testing-nestjs/tree/main/apps/typeorm-sample)  
[Dominion Solutions Blog - Mocking TypeORM with Jest](https://dominion.solutions/blog/2021-06-20/mocking-typeorm-with-jest/#articles)  
  
  
Solution - Mocking The Correct Compenents  
After much testing, and looking at different approaches, the solution finally showed up in the form of mocking the correct components.  
  
  
```ts  
import { User } from '@dominion-solutions/user-permission';  
import * as faker from 'faker';  
import { loginHandler } from '../handler';  
import * as jwt from 'jsonwebtoken';  
import * as dotenv from 'dotenv';  
import { fail } from 'assert';  
import Sinon from 'sinon';  
import { BaseEntity, Connection, ConnectionManager, Repository } from 'typeorm';  
import 'jest-extended'  
  
describe('handler => loginHandler()', () => {  
    const sandbox = Sinon.createSandbox();  
    const goodUser: User = new User;  
    const password: string = faker.internet.password();  
    goodUser.userName = faker.internet.userName();  
    beforeAll(async () => {  
        dotenv.config();  
        await goodUser.setPassword(password);  
  
    });  
  
    beforeEach(() => {  
        const mockRepository = sandbox.stub(Repository.prototype, 'find').returns(Promise.resolve([goodUser]));  
  
        sandbox.stub(ConnectionManager.prototype, 'get').returns({  
            getRepository: sandbox.mock().returns(mockRepository)  
          } as unknown as Connection);  
        sandbox.stub(BaseEntity, 'find').returns(Promise.resolve([goodUser]));  
    });  
  
    afterEach(() => {  
        sandbox.restore();  
    });  
```  
  
  
  
- TypeOrm Querybuilder 테스트 코드  
  
```ts  
export const mockRepository = () => ({  
  create: jest.fn(),  
  save: jest.fn(),  
  update: jest.fn(),  
  createQueryBuilder: jest.fn().mockReturnValue({  
    update: jest.fn().mockReturnThis(),  
    set: jest.fn().mockReturnThis(),  
    where: jest.fn().mockReturnThis(),  
    execute: jest.fn().mockReturnThis(),  
  }),  
});  
  
```  
  
[Unit Test for a Query Builder · Issue #1774 · typeorm/typeorm · GitHub](https://github.com/typeorm/typeorm/issues/1774)  
  
- [SpyOn TypeORM repository to change the return value for unit testing NestJS – iTecNote](https://itecnote.com/tecnote/spyon-typeorm-repository-to-change-the-return-value-for-unit-testing-nestjs/)  
[NestJS 11 | Unit Test - QueryBuilder & Transaction](https://velog.io/@hkja0111/NestJS-11-Unit-Test-QueryBuilder)  
  
```ts  
  async resetRefreshToken(id: string): Promise<void> {  
    //  유저가 로그아웃 할 때 사용  
    // TODO: 에러 핸들링  
    await this.userRepository  
      .createQueryBuilder()  
      .update(User)  
      .set({ hashedRefreshToken: null })  
      .where('id = :id', { id })  
      .execute();  
  }  
```  
  
테스트 하고 싶을 때  
```ts  
    it('reset null refrech token in database', () => {  
      const userId = 'uuid';  
      authService.resetRefreshToken(userId);  
  
      expect(mockedUserRepository.createQueryBuilder).toHaveBeenCalled();  
      expect(  
        mockedUserRepository.createQueryBuilder().update().set,  
      ).toHaveBeenCalledWith({ hashedRefreshToken: null });  
      expect(mockedUserRepository.createQueryBuilder().execute).toBeCalledTimes(  
        1,  
      );  
    });  
```  
# 오류  
## Cannot Find Module 에러  
[typescript - NestJS - Test suite failed to run Cannot find module 'src/article/article.entity' from 'comment/comment.entity.ts' - Stack Overflow](https://stackoverflow.com/questions/63865678/nestjs-test-suite-failed-to-run-cannot-find-module-src-article-article-entity)  
![Pasted image 20230105231400.png](../../attachments/Pasted%20image%2020230105231400.png)  
  
  
  
---  
  
# E2E Test  
  
  
# Redis Test  
[https://github.dev/alkem-io/server/blob/e1a23a664904a7f2740d3310829e1818e5700350/test/mocks/cache-manager.mock.ts](https://github.dev/alkem-io/server/blob/e1a23a664904a7f2740d3310829e1818e5700350/test/mocks/cache-manager.mock.ts)  
  
  
[How to mock/stub/change CacheModule.store option for a test app ? · Issue #681 · nestjs/docs.nestjs.com · GitHub](https://github.com/nestjs/docs.nestjs.com/issues/681#issuecomment-1096939527)  
  
- Cache-manager.mock.ts  
```ts  
```  
  
  
  
  
- 모킹 리셋해주는 법  
[Jest set, clear and reset mock/spy/stub implementation · Code with Hugo](https://codewithhugo.com/jest-stub-mock-spy-set-clear/)  
  
  
```ts  
// mock + code under test definition  
beforeEach(() => {  
  jest.clearAllMocks();  
});  
// tests  
```  
  
# Test  
[test code](test%20code.md)  
[Ultimate Guide: NestJS Unit Testing and Mocking [Updated 2022]](https://www.tomray.dev/nestjs-unit-testing#automating-nestjs-unit-tests-in-a-ci-pipeline-with-github-actions)  
  
[Unit testing NestJS applications with Jest - LogRocket Blog](https://blog.logrocket.com/unit-testing-nestjs-applications-with-jest/)  
  
완전 좋은 참고 코드  
[GitHub - hantsy/nestjs-rest-sample: NestJS RESTful APIs Sample](https://github.com/hantsy/nestjs-rest-sample)  
  
  
# Husky 붙여보기  
[Husky setting](../../../Husky%20setting.md)  
git hook 사용한 자동 동작실행하는 패키지  
원하는 스크립트 실행시켜줌  
  
  
nest Js 관련 쓸만한 마이크로서비스  정리한 블로그  
[Best way to baseline nestjs microservice – @tkssharma | Tarun Sharma | My Profile](https://tkssharma.com/nestjs-microservice-baseline-setup/)  
  
  
## Uuid Mocking  
```ts  
import { v4 as uuidv4 } from 'uuid';  
  
  
jest.mock('uuid', () => ({ v4: () => 'mocked-uuid' }));  
  
  
  
  describe('mock uuid', () => {  
    it('should return testId', () => {  
      const uuid = uuidv4();  
      expect(uuid).toBe('mocked-uuid');  
  
      const uuid2 = uuidv4();  
      expect(uuid2).toBe('mocked-uuid');  
    });  
  });  
  
  it('should be defined', () => {  
    expect(authService).toBeDefined();  
  });  
  
```  
  
  
참고 깃허브 코드  
- [https://github.dev/thisismydesign/nestjs-starter](https://github.dev/thisismydesign/nestjs-starter)  
- [https://github.dev/toondaey/nestjs-oauth2-server](https://github.dev/toondaey/nestjs-oauth2-server)  
- [https://github.dev/jmcdo29/testing-nestjs](https://github.dev/jmcdo29/testing-nestjs)  
- [https://github.dev/hantsy/nestjs-rest-sample](https://github.dev/hantsy/nestjs-rest-sample)  
  
# 트랜잭션 테스트  
  
[[NestJS] TypeORM — Relation, Transactions | by JeungJoo Lee | CrocusEnergy | Medium](https://medium.com/crocusenergy/nestjs-typeorm-relation-transactions-bcf5c1fb9942)  
  
[How to mock typeorm connection using jest · Issue #5751 · typeorm/typeorm · GitHub](https://github.com/typeorm/typeorm/issues/5751#issuecomment-1414309716)  
  
  
# TypeOrm 1:N 관계 어떻게 모킹해서 테스트함  
  
  
  
  
# Run Jest Tests Only for Current Folder  
[javascript - Run Jest tests only for current folder - Stack Overflow](https://stackoverflow.com/questions/49613127/run-jest-tests-only-for-current-folder)  
  
  
  
> 시발~ 레포지토리 모킹 이런거 필요없고 인풋아웃풋 제대로 받나 대충 쉽게 생각하면 됨~  
  
# 초기화  
[javascript - How to reset or clear a spy in Jest? - Stack Overflow](https://stackoverflow.com/questions/53350382/how-to-reset-or-clear-a-spy-in-jest)  
```ts  
afterEach(() => {      
  jest.clearAllMocks();  
});  
```  
  
  
  
  
테스트 정리 완전 잘되어있음  
[Ultimate Guide: NestJS Unit Testing and Mocking](https://www.tomray.dev/nestjs-unit-testing)