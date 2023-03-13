---  
category: Project/Testing  
share: true  
---  
# 참고 페이지.. (내일 본격적으로 테스트 코드 써보기)  
  
> [https://dev.to/tkssharma/unit-testing-and-integration-testing-nestjs-application-4d7a](https://dev.to/tkssharma/unit-testing-and-integration-testing-nestjs-application-4d7a)  
  
`Fakes`: an object with limited capabilities (for the purposes of testing), e.g. a fake web service. Fake has business behavior. You can drive a fake to behave in different ways by giving it different data. Fakes can be used when you can’t use a real implementation in your test.  
  
`Mock`: an object on which you set expectations. A mock has expectations about the way it should be called, and a test should fail if it’s not called that way. Mocks are used to test interactions between objects.  
  
`Stub`: an object that provides predefined answers to method calls. A stub has no logic and only returns what you tell it to return.    
In case you are interested, here is a good discussion on fake/mock/stub.  
  
`Spy`: Spy, spies on the caller. Often used to make sure a particular method has been called.  
  
그 외 내일 볼 페이지: [tests for the AuthenticationService](https://wanago.io/2020/07/06/api-nestjs-unit-tests/)    
노마드 코더 이분 영상에도 유닛 테스트 관련 영상이 있어서 내일 보자.    
귀찮으니까 정리해두신 분 [https://velog.io/@qmasem/TIL-노마드-코더-NestJS로-API-만들기-4](https://velog.io/@qmasem/TIL-%EB%85%B8%EB%A7%88%EB%93%9C-%EC%BD%94%EB%8D%94-NestJS%EB%A1%9C-API-%EB%A7%8C%EB%93%A4%EA%B8%B0-4) // 보면서 하기    
[https://wikidocs.net/158681](https://wikidocs.net/158681)    
[https://lab.cliel.com/entry/nestjs-spec%EA%B3%BC-testing](https://lab.cliel.com/entry/nestjs-spec%EA%B3%BC-testing)    
[https://dailybook-with.tistory.com/entry/Nestjs-%EC%9D%98-%EC%9C%A0%EB%8B%9B-%ED%85%8C%EC%8A%A4%ED%8A%B8Unit-test](https://dailybook-with.tistory.com/entry/Nestjs-%EC%9D%98-%EC%9C%A0%EB%8B%9B-%ED%85%8C%EC%8A%A4%ED%8A%B8Unit-test)  
  
  
# 공식 문서 보고 따라해보기  
[Testing | NestJS - A progressive Node.js framework](https://docs.nestjs.com/fundamentals/testing)  
[18.1 소프트웨어 테스트 - NestJS로 배우는 백엔드 프로그래밍 (wikidocs.net)](https://wikidocs.net/158681)  
  
[Unit Testing: Jest with NestJS. NestJS is a powerful backend framework… | by DLT Labs | DLT Labs | Medium](https://medium.com/dlt-labs-publication/unit-testing-jest-with-nestjs-b5b043ac5598)  
[Getting started with continuous integration for Nest.js APIs | CircleCI](https://circleci.com/blog/getting-started-with-nestjs-and-automatic-testing/)  
  
We need to create a “spec” file to define the unit tests. That file would normally be named cats.controller.spec.ts. “Spec” files may be given a different extension, but that must be configured in Jest configuration.  
우리는 유닛 테스트를 진행하기 위해서 spec파일을 만들어야한다 (cli를 사용해서 파일을 생성하면 spec 만들어주는데 난 삭제해버림... 귀찮아서 확장프로그램 깔았더니 알아서 코드 쳐줬다 !!)  
`Spec` 파일은   
  
![스크린샷 2022-09-26 오후 3.47.46.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-26%20%EC%98%A4%ED%9B%84%203.47.46.png)  
`pagckage.json` 파일에 보면 jest 설정이 이렇게 되어있다.  
  
[Jest CLI Options · Jest (jestjs.io)](https://jestjs.io/docs/cli) 에서 보면 *Watch mode* also enables to specify the name or path to a file to focus on a specific set of tests. 라고 한다.  
  
```  
jest --watch #runs jest -o by default  
jest --watchAll #runs all tests  
```  
  
  
![스크린샷 2022-09-26 오후 5.24.23.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-26%20%EC%98%A4%ED%9B%84%205.24.23.png)  
  
<iframe width="560" height="315" src="https://www.youtube.com/embed/Gjv_IgcyLBo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  
  
## Jest Mocking  
  
```typescript  
const userRepositorySaveSpy = jest  
  .spyOn(userRepository, 'save')  
  .mockResolvedValue(savedUser);  
```  
  
  
> [Jest](https://www.npmjs.com/package/jest)에서는 모킹(mocking) 함수들을 제공하고 있다. Mock은 단위 테스트를 작성할 때, 해당 코드가 의존하는 부분을 가짜(mcok)로 대체하는 기법이다. 일반적으로는 테스트하려는 코드가 의존하는 부분을 직접 생성하기가 너무 부담스러울 때 Mock이 사용된다. `jest.spyOn`은 `jest.fn`과 유사한 모의 함수를 만들지만 함수 호출을 추적할 수 있다는 점에서 다르다. 위 코드에서는 `spyOn`으로 `userRepository`의 `save` 함수 호출을 모의하고 이 모의된 함수는 `mockResolvedValue`를 사용해서 `savedUser`를 반환하도록 정의하고 있다.  
출처: [NestJS에서 단위 테스트 작성하기 — JHyeok](https://jhyeok.com/nestjs-unit-test/)  
  
  
Now that our API is working as expected, in this section, we’ll focus on writing tests for the methods defined in the `ProductService` class that was created earlier. It feels appropriate to only test this part of the application as it handles most of the business logic.  
  
Nest.js comes with a built-in testing infrastructure, which means we don’t have to set up much configuration in terms of testing. Though Nest.js is agnostic to testing tools, it provides integration with [Jest](https://jestjs.io/) out of the box. Jest will provide assert functions and test-double utilities that help with mocking.  
  
  
# ## Writing a test for the ‘create’ methods  
  
Remember, we did not start this project using the **test-driven development** approach. So, we’ll write the tests to ensure that all business logic within the `ProductService` receives the appropriate parameters and return the expected response. To start, open the `product.service.spec.ts` file and replace its content with this:  
  
```ts  
import { Test, TestingModule } from '@nestjs/testing';  
  
import { BlogService } from '../src/blog/blog.service';  
  
    
  
describe('BlogService', () => {  
  
let blogService;  
  
    
  
beforeEach(async () => {  
  
const module: TestingModule = await Test.createTestingModule({  
  
providers: [BlogService],  
  
}).compile();  
  
    
  
blogService = module.get<BlogService>(BlogService);  
  
});  
  
    
  
it('should be defined', () => {  
  
expect(blogService).toBeDefined();  
  
});  
  
});  
```  
  
1. describe는 테스트를 묘사한다는 뜻 [Configuring Jest · Jest (jestjs.io)](https://jestjs.io/docs/configuration)  
2. beforeEach는 테스트를 하기 전 실행하는 로직  
3. it은 테스트를 하는 로직  
  
자세한 사항은 [Configuring Jest · Jest (jestjs.io)](https://jestjs.io/docs/configuration) 공식 문서를 보면 된다.  
  
  
  
```ts  
import { Test, TestingModule } from '@nestjs/testing';  
  
import { BlogController } from './blog.controller';  
  
import { BlogService } from './blog.service';  
  
    
  
describe('BlogService', () => {  
  
let blogService: BlogService;  
  
let blogController: BlogController;  
  
    
  
beforeEach(async () => {  
  
const module: TestingModule = await Test.createTestingModule({  
  
controllers: [BlogController],  
  
providers: [BlogService],  
  
}).compile();  
  
    
  
blogService = module.get<BlogService>(BlogService);  
  
});  
  
    
  
it('should be defined', () => {  
  
expect(blogService).toBeDefined();  
  
});  
  
    
  
// it('should be 4', () => {  
  
// expect(2 + 2).toEqual(4);  
  
// });  
  
});  
```  
  
이렇게 쳤더니 ![스크린샷 2022-09-26 오후 5.43.29.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-26%20%EC%98%A4%ED%9B%84%205.43.29.png)이런 오류가...  구글링해봤더니  
[jestjs - Nest can't resolve dependencies in the RootTestModule context when I use Bazel Test - Stack Overflow](https://stackoverflow.com/questions/62942112/nest-cant-resolve-dependencies-in-the-roottestmodule-context-when-i-use-bazel-t) 이런 문서가 나왔다.  
  
> Add providers to your RootTestModule. Nest doesn't automatically include the services in your test, depending on if you used the cli vs creating the files/folders directly.  
  
  
[jmcdo29/testing-nestjs: A repository to show off to the community methods of testing NestJS including Unit Tests, Integration Tests, E2E Tests, pipes, filters, interceptors, GraphQL, Mongo, TypeORM, and more! (github.com)](https://github.com/jmcdo29/testing-nestjs)  
오 좋은 예제 깃허브 찾음!! 여기 참고하면서 써봐야지!!  
  
  
아니 이제 커스텀 repositorty 사라지면서 따로 파일 불러오는게 안되는데..   
  
![스크린샷 2022-09-26 오후 7.55.54.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-26%20%EC%98%A4%ED%9B%84%207.55.54.png)  
repository가 없어서 주입해줘야 하는 문제였는듯   
  
  
blogService는 repository가 필요한데, test Module에서 repository를 제공하지 않아 생기는 문제dla  
  
그렇다고 TypeORM Module의 Repository를 제공하지 않습니다. 저희는 **Mock Repository**를 제공하면 됨!  
  
  
[[NestJs 에러] Nest can't resolve dependencies of the Service (?). Please make sure that the argument Repository at index [0] is available in the ResultIoModule (tistory.com)](https://junior-datalist.tistory.com/147)  
참고사이트  
  
이렇게 하니까 해결됐다  
  
[ service.spec.ts 깃허브 링크)](https://github.com/baksohyeon/making-blog/commit/cf3d7b05f69d59b0c07187db8a535ae1be198806)  
  
  
  
# 가장 간단한 테스트 기능 써보기  
  
![스크린샷 2022-09-26 오후 7.03.42.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-26%20%EC%98%A4%ED%9B%84%207.03.42.png)  
  
  
![스크린샷 2022-09-26 오후 7.41.25.png](../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-26%20%EC%98%A4%ED%9B%84%207.41.25.png)  
  
이캐 됨!  
  
  
## ## Writing a test for the ‘create’ and ‘get’ products methods  
  
  
---  
# Mock Functions  
  
Mock functions are also known as "spies", because they let you spy on the behavior of a function that is called indirectly by some other code, rather than only testing the output. You can create a mock function with `jest.fn()`. If no implementation is given, the mock function will return `undefined` when invoked.  
[Mock Functions · Jest (jestjs.io)](https://jestjs.io/docs/mock-function-api)  
  
  
  
## jest.spyOn(object, methodName)  
[The Jest Object · Jest (jestjs.io)](https://jestjs.io/docs/jest-object#jestspyonobject-methodname)  
  
  
  
[Unit testing NestJS applications with Jest - LogRocket Blog](https://blog.logrocket.com/unit-testing-nestjs-applications-with-jest/)  
  
  
JEST 구조  
![Pasted image 20220926225656.png](../../attachments/Pasted%20image%2020220926225656.png)  
  
Mock 개념 알기   
[Mock - 인코덤, 생물정보 전문위키 (incodom.kr)](http://www.incodom.kr/Mock)