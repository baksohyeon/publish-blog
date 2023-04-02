---
category: Project/Testing
share: true
title: Making Test Code at Controller Layer
---

# Making Test Code at Controller Layer
[what is getRepositoryToken in nestjs typeorm and when to use it?](https://stackoverflow.com/questions/65570680/what-is-getrepositorytoken-in-nestjs-typeorm-and-when-to-use-it)
[ ] [Database | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/database#testing) 문서


> When it comes to unit testing an application, we usually want to avoid making a database connection, keeping our test suites independent and their execution process as fast as possible. But our classes might depend on repositories that are pulled from the connection instance. How do we handle that? **The solution is to create mock repositories.** In order to achieve that, we set up custom providers. Each registered repository is automatically represented by a Repository token, where EntityName is the name of your entity class.
> The @nestjs/typeorm package exposes the `getRepositoryToken()` function which returns a prepared token based on a given entity.

`getRepositoryToken()` is a helper method that allows you to get the ==same injection token== that **@InjectRepository()** returns. 
This is useful when it comes to tests so that you can define a custom provider that has a matching token for the DI resolution, and so you can provide a mock of the Repository methods without the need to actually talk to the database. So for example, if you have

```ts
@Injectable()
export class FooService {
  constructor(@InjectRepository(Foo) private readonly fooRepo: Repository<Foo>) {}

}
```


In your test you can add the provider
```ts
{
  provide: getRepositoryToken(Foo),
  useValue: {
    find: jest.fn(),
    insert: jest.fn(),
  },
}
```

And now you've got a mock injectable provider for the Repository.

The biggest reason that things have to be done this way is because typescript doesn't reflect generic classes, it only reflects Repository, and if Nest tries to figure out which repository you mean to inject, with just that name (Repository) it's most likely going to get it wrong and inject the wrong class. Using `@InjectRepsitory()` allows for setting the proper injection token.

```ts

  const BOARD_REPOSITORY_TOKEN = getRepositoryToken(Board);
  console.log(BOARD_REPOSITORY_TOKEN);

```


![[스크린샷 2022-09-27 오후 2.56.05.png]]
콘솔 찍어보면 레포지토리 나옴
It's how nestjs can find the dependency from the container 

# `module.get()`
![[스크린샷 2022-09-27 오후 3.38.48.png]]
This method actually takes in a type, or token.
You can actually either pass in an actual token to retrieve the actual dependency or you can pass in the type.

you're really actually trying to retrieve the dependency from the actual contianer because remember nest js is based on dependency injection.
So there's a container that has all of the dependencies 
When you declare and initialize all of its dependencies during runtime and then you can retrieve those dependencies from the container



# beforeEach 코드 이해
```ts
  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [BlogController],
      providers: [
        {
          provide: BlogService,
          useValue: mockBlogService(),
        },
      ],
    }).compile();

    blogService = module.get<BlogService>(BlogService);
    blogController = module.get<BlogController>(BlogController);

    console.log(blogService);
  });
```

## Test Class
The `Test` class is useful for providing an application execution context that essentially mocks the full Nest runtime, but gives you hooks that make it easy to manage class instances, including mocking and overriding. 
![[스크린샷 2022-09-27 오후 7.16.35.png]]
The `Test` class has a `createTestingModule()` method that takes a module metadata object as its argument (the same object you pass to the` @Module()` decorator). 

This method returns a TestingModule instance which in turn provides a few methods. For unit tests, the important one is the `compile()` method. 
This method bootstraps a module with its dependencies (similar to the way an application is bootstrapped in the conventional `main.ts` file using `NestFactory.create()`), and returns a module that is ready for testing.

> HINT
The compile() method is **asynchronous** and therefore has to be awaited. 
Once the module is compiled you can retrieve any static instance it declares (controllers and providers) using the `get()` method.

> `get()`: Retrieves a static instance of a controller or provider (including guards, filters, etc.) available in the application context. Inherited from the module reference class.

You can also retrieve these mocks out of the testing container as you normally would custom providers, `moduleRef.get(CatsService)`.



## TestingModule
TestingModule inherits from the module reference class, and therefore its ability to dynamically resolve scoped providers (transient or request-scoped). Do this with the resolve() method (the `get()` method can only retrieve static instances).

Instead of using the production version of any provider, you can **override** it with a custom provider for testing purposes. 
For example, you can mock a database service instead of connecting to a live database. We'll cover overrides in the next section, but they're available for unit tests as well.

![[스크린샷 2022-09-27 오후 7.20.36.png]]

Similarly, Nest provides methods to override guards, interceptors, filters and pipes with theoverrideGuard(), overrideInterceptor(), overrideFilter(), and overridePipe() methods respectively.

Each of the override methods returns an object with 3 different methods that mirror those described for custom providers:

- `useClass`: you supply a class that will be instantiated to provide the instance to override the object (provider, guard, etc.).
- `useValue`: you supply an instance that will override the object.
- `useFactory`: you supply a function that returns an instance that will override the object.
	Each of the override method types, in turn, returns the TestingModule instance, and can thus be chained with other methods in the fluent style. You should use compile() at the end of such a chain to cause Nest to instantiate and initialize the module.

![[스크린샷 2022-09-27 오후 4.10.38.png]]

더 모듈 관련 자세히 알고싶으면 https://docs.nestjs.com/fundamentals/module-ref 여기 참고하면 됨


# [Jest] jest.fn(), jest.spyOn() 함수 모킹
https://www.daleseo.com/jest-fn-spy-on/

## jest.spyOn() 사용법
https://www.daleseo.com/jest-fn-spy-on/
```Ts
const calculator = {
  add: (a, b) => a + b,
};

const spyFn = jest.spyOn(calculator, "add");

const result = calculator.add(2, 3);

expect(spyFn).toBeCalledTimes(1);
expect(spyFn).toBeCalledWith(2, 3);
expect(result).toBe(5);
```
도청기 붙이는 느낌

# 컨트롤러 테케 뼈대 만들기

# 컨트롤러 테케 오류 해결
## 1. Defined가 안됨

```ts
import { Test, TestingModule } from '@nestjs/testing';
import { getRepositoryToken } from '@nestjs/typeorm';
import { BlogController } from '../blog.controller';
import { BlogService } from '../blog.service';
import { CreateBoardDto } from '../dto/create-board.dto';

describe('BlogController', () => {
  let blogController: BlogController;
  let blogService: BlogService;

  const mockBlogService = () => {
    createBoard: jest
      .fn()
      .mockImplementation((createBoardDto: CreateBoardDto) =>
        Promise.resolve({ id: 'a uuid', dated_at: 'date', ...createBoardDto }),
      );
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [BlogController],
      providers: [
        {
          provide: BlogService,
          useValue: mockBlogService(),
        },
      ],
    }).compile();

    blogService = module.get<BlogService>(BlogService);
    blogController = module.get<BlogController>(BlogController);
  });

  it('shoud be defined', () => {
    expect(blogController).toBeDefined();
  });
});

describe('createBoard', () => {});

```
문제의 코드
![[스크린샷 2022-09-27 오후 5.31.38.png]]
This failed and the reason why is it's telling us if blog service is a provider, is it part of the root test module.

So this is where we have have a dependency in our controller and what we need to do is we need to let the module know that we have a dependency 


`해결` mockBlogService 세팅을 잘못해줬던거였음
```ts
 const mockBlogService = () => {
    createBoard: jest
      .fn()
      .mockImplementation((createBoardDto: CreateBoardDto) =>
        Promise.resolve({ id: 'a uuid', dated_at: 'date', ...createBoardDto }),
      );
  };
```
라고 선언했는데 내가 쓴게 틀렸음 

```ts
 const mockBlogService = () => ({
    createBoard: jest
      .fn()
      .mockImplementation((createBoardDto: CreateBoardDto) =>
        Promise.resolve({
          id: 'a uuid',
          dated_at: 'date',
          ...createBoardDto,
        }),
      ),
  });

```

## getAllBoards 테케 만들기
```ts
describe('BlogController', () => {
  let blogController: BlogController;
  let blogService: BlogService;

  const mockBlogService = () => ({
    // ... ///

     getAllBoards: jest.fn().mockResolvedValue([
      {
        title: '1',
        description: '1',
        body: '1',
        author: '1',
      },
      {
        title: '2',
        description: '2',
        body: '2',
        author: '2',
      },
      {
        title: '3',
        description: '3',
        body: '3',
        author: '3',
      },
    ]),
  });

// beforeEach .. //

  describe('getAllboards', () => {
    it('should be get an array of boards', async () => {
      await expect(blogController.getAllBoards()).resolves.toEqual([
        {
          title: '1',
          description: '1',
          body: '1',
          author: '1',
        },
        {
          title: '2',
          description: '2',
          body: '2',
          author: '2',
        },
        {
          title: '3',
          description: '3',
          body: '3',
          author: '3',
        },
      ]);
    });
  });

```

`mockResolvedValue(Promise가 resolve하는 값)` 함수를 이용하면 가짜 비동기 함수

## getBoardsByAuthor 테케 만들기
```ts
describe('BlogController', () => {
  let blogController: BlogController;
  let blogService: BlogService;

  const mockBlogService = () => ({
    // ... ///

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
`mockImplementation(구현 코드)` 함수를 이용하면 아예 해당 함수를 통째로 가리고 mock구현체를 바라보게 됨

`Promise.resolve()`
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve
https://jestjs.io/docs/asynchronous













## beforeEach()
https://jestjs.io/docs/setup-teardown
Often while writing tests you have some setup work that needs to happen before tests run, and you have some finishing work that needs to happen after tests run. Jest provides helper functions to handle this.
```ts
beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [BlogController],
      providers: [
        {
          provide: BlogService,
          useValue: mockBlogService(),
        },
      ],
    }).compile();

    blogService = module.get<BlogService>(BlogService);
    blogController = module.get<BlogController>(BlogController);
  });
```


`beforeEach` and `afterEach` can handle asynchronous code in the same ways that tests can handle asynchronous code(https://jestjs.io/docs/asynchronous) - they can either take a done parameter or return a promise. For example, if `initializeCityDatabase()` returned a promise that resolved when the database was initialized, we would want to return that promise:


Jest executes all describe handlers in a test file before it executes any of the actual tests. This is another reason to do setup and teardown inside `before*` and `after*` handlers rather than inside the `describe` blocks. Once the `describe` blocks are complete, by default Jest runs all the tests serially in the order they were encountered in the collection phase, waiting for each to finish and be tidied up before moving on.


https://www.daleseo.com/jest-before-after/

---
# 서비스코드 테케 뼈대
```TS
// blog.service.spec.ts

import { Test, TestingModule } from '@nestjs/testing';
import { getRepositoryToken } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { BlogController } from '../blog.controller';
import { BlogService } from '../blog.service';
import { Board } from '../entity/board.entity';

const mockBlogRepository = () => ({
  create: jest.fn(),
  find: jest.fn(),
  save: jest.fn(),
});

describe('BlogService', () => {
  let blogService: BlogService;
  let blogRepository: Repository<Board>;

  const BOARD_REPOSITORY_TOKEN = getRepositoryToken(Board);

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        BlogService,
        {
          provide: BOARD_REPOSITORY_TOKEN,
          useValue: mockBlogRepository(),
        },
      ],
      controllers: [BlogController],
    }).compile();

    blogService = module.get<BlogService>(BlogService);
    blogRepository = module.get<Repository<Board>>(BOARD_REPOSITORY_TOKEN);
  });

  it('should be defined', () => {
    expect(blogService).toBeDefined();
  });
});

```