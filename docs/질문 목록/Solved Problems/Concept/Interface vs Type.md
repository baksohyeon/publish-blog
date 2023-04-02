---
category: 질문 목록/Solved Problems/Concept
share: true
title: Interface vs Type
---

# Interface Vs Type

## 질문 내용

유데미에서 반환하는 타입 정할 때 코드 이렇게 쓰는데 대체 먼 문법임

```ts
import { User } from "./entity/user.entity";

export interface UserResponseInterface {
  user: User & { token: string };
}
```

저 코드가 원하는 것: 유저 entity에 토큰 타입을 추가해서 반환하고 싶음
근데 & 연산자가 뭐하는 용법임?

강의에서 저렇게 쓰던데 저캐 써도 됨?
User는 클래스 객체구... & 연산자 다음은 객체인데.. 인터페이스로 반환.. 머임

더 나은 코드가 있지 않을까?

## 질문 관련 서치 내용

[What are intersection types in Typescript ? - GeeksforGeeks](https://www.geeksforgeeks.org/what-are-intersection-types-in-typescript/)
[TypeScript/spec-ARCHIVED.md at v4.5.4 · microsoft/TypeScript · GitHub](https://github.com/microsoft/TypeScript/blob/v4.5.4/doc/spec-ARCHIVED.md#35-intersection-types)
[Typescript & operator - Stack Overflow](https://stackoverflow.com/questions/33875609/typescript-operator)
[TypeScript: Documentation - Interfaces vs. Intersections](https://www.typescriptlang.org/docs/handbook/2/objects.html#interfaces-vs-intersections)
[Master Interfaces In Typescript · Nicotsou](https://nicotsou.com/tltr-typescript-interfaces/)

## 질문을 정리하면서 새롭게 얻은 해답이 있는가?

```js
interface A {
  a: number;
}
interface B {
  b: number;
}

var ab: A & B = { a: 1, b: 1 };
var a: A = ab; // A & B assignable to A
var b: B = ab; // A & B assignable to B

interface X {
  p: A;
}
interface Y {
  p: B;
}

var xy: X & Y = { p: ab }; // X & Y has property p of type A & B

type F1 = (a: string, b: string) => void;
type F2 = (a: number, b: number) => void;

var f: F1 & F2 = (a: string | number, b: string | number) => {};
f("hello", "world"); // Ok
f(1, 2); // Ok
f(1, "test"); // Error
```

The ‘&’ operator is used to create the intersection type.

Intersection types represent values that simultaneously have multiple types. A value of an intersection type A & B is a value that is both of type A and type B. Intersection types are written using intersection type literals (section 3.8.7).

```Ts
interface A { a: number }
interface B { b: number }

var ab: A & B = { a: 1, b: 1 };
var a: A = ab;  // A & B assignable to A
var b: B = ab;  // A & B assignable to B
```

Because ab is both of type A and of type B, we can assign it to a and/or b. If ab were only of type B, we could only assign it to b.

```ts
// base type
interface Shape {
  color: string;
}

// extension
interface Square extends Shape {
  sideLength: number;
}

// intersection
type Square = Shape & {
  sideLength: number;
};
```

클래스는 주형틀 기능 있는 객체..
인터페이스는 타입 선언만 해주고 어떻게 생겻는지만 정해주는건데..
인터페이스는 타스에만 잇어서 자스로 바꾸면 날라가니까 가급적 네스트에서는 인터페이스 지양해라... 로 알고있음

## 추가로 찾아본 것

### Intersection, Extention 차이

[types - Difference between extending and intersecting interfaces in TypeScript? - Stack Overflow](https://stackoverflow.com/questions/52681316/difference-between-extending-and-intersecting-interfaces-in-typescript)

the most significant is the difference in how members with the same property key are handled when present in both types.

pointing out the difference in behaviour when 'overriding' properties

"The principle difference between the two is how conflicts are handled, and that difference is typically one of the main reasons why you’d pick one over the other between an interface and a type alias of an intersection type."

**뭔 소리지**

> 질문하기

이게 객체의 형태가 유지되었을 때 타입 차원에서 이것저것 다양하게 할 수 있는 게 많아지는데 인터섹션이 한 번 섞이는 순간 이게 다 날라가 버립니다.

intersection은 오버라이딩 잘 되고 extends는 안된다로만 이해해도 됨
그리고 대부분 extends로 씀

### 걍 Class 로 만들어서 상속받으면 안댐..?

개념 하나도 모르는건강
class - extends 상속 개념 더 찾아보기

[ES6의 Class extends 내부 동작원리에 대해서 설명해보세요 | 2ssue's dev note](https://2ssue.github.io/common_questions_for_Web_Developer/docs/Javascript/8_es6_class_extends.html)
[[JavaScript] 상속 (extends, super()) — 나보단 잘하자](https://jae04099.tistory.com/entry/JavaScript-%EC%83%81%EC%86%8D-extends-super)

### Type, Interface 차이

[[타입스크립트] 인터페이스 vs 타입 에 대하여](https://all-dev-kang.tistory.com/entry/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-vs-%ED%83%80%EC%9E%85-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)

바로 interface는 extends로 확장하고 type은 &(Intersection)으로 추가하면 됩니다.

그렇다면 type과 interface의 차이점은 무엇일까?

첫 번째로는 조합이라고 볼 수 있습니다.

type은 유니온, 인터섹션 등 타입끼리의 연산이 가능하다는 것입니다. 따라서 이러한 이점은 타입스크립트에서 타입을 선언하는 데에 빠른 속도를 제공합니다. 특정 타입을 빼버리거나 추가하는 게 상대적으로 간편하고 타입스크립트에서도 이러한 기능을 제공하고 있습니다.

type과 interface의 기본적인 기능 차이는 거의 없다고 생각합니다. 따라서 더 편한 것을 써도 됩니다.

하지만 위에서 설명한대로 코드의 일관성과 확장의 여부를 신경써야합니다.

> type : 개별 또는 내부적으로 선언되는 타입, 타입 내부의 변경보다는 다른 타입끼리의 조합등에 사용될 경우
> Iinterface : 변경 가능성이 높은 타입, api의 input, output 값처럼 외부로 연결 또는 확장되는 경우

### Union `|`

[TypeScript Intersection Types](https://www.typescripttutorial.net/typescript-tutorial/typescript-intersection-types/)

Note that the union type uses the | operator that defines a variable which can hold a value of either typeA or typeB

말 그대로 객체의 키를 union으로 가져오는 연산자

## Key of 는 뭐지

[TypeScript: Documentation - Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

## 추가 질문 및 답변

1. Intersection, Extention 차이

일단 `Intersection`은 **type**으로 선언된 타입들에 대해 타입을 확장하기 위해 사용되구요, & 연산자를 사용합니다
`Extension`은 **interface**로 선언된 타입들에 대해 프로퍼티를 확장하기 위해 사용되구요, extends를 통해 기반이 될 인터페이스를 선언하는 방식으로 사용합니다.

일단 먼저 Extension은 객체 형태의 타입을 interface에서 확장할 때에 사용하구요
반면 Intersection은 객체 형태의 타입이 아니어도 사용할 수 있는데요

1. interface 도 & 로 확장 가능하다는게 맞다는건지 안된다는건지
   type이랑 비슷한거니까 저렇게 써도 된다는건가요..?
   (아니 그럼 저 강의속 코드 잘못된 것 아님? → extends 써야하는 것 아닌가)

일단 interface 자체는 Intersection(&)로는 확장이 안됨
근데 저거는 interface 내에 들어가는 개별 필드에 Intersection이 적용된 거라서 상관없어요!
그리니까 풀어 써보자면...

```ts
import { User } from "./entity/user.entity";

export interface UserResponseInterface {
  user: UserType;
}
type UserType = User & { token: string };
```

라서 괜찮음~

인터페이스는 객체
인터페이스에 드러가는 객체들(= 속성)에 타입을 지정해주는데 그 타입들에 intersection이 걸린다.

1. Type, Interface 차이에 대해서, 실제 협업 입장에선 어떻게 바라보는지 (중요도가 있는 건가요?)
   type이 대다수의 상황에서 훨씬 강력해요
   예를 들면 type을 사용하면.....

```ts
type Awaited<T extends Promise<unknown>> = T extends Promise<infer O>
  ? O
  : never;
type ReturnType<T extends (...args: unknown[]) => unknown> = T extends (
  ...args: unknown[]
) => infer O
  ? O
  : never;
const result: Awaited<ReturnType<typeof getResult>> = await getResult();
```

뭐 이런 식으로 괴상한 것들을 활용해서 타입을 가지고 놀 수 있음
(위 코드는 함수의 반환 타입을 뽑아내는 타입과, Promise의 반환 타입을 뽑아내는 타입을 작성한 것입니다)
반면 인터페이스는 그냥 객체
이거가 장점이 될 수도 단점이 될 수도 있는데
장점이라고 하는 사람들은 확장 여지가 제한되어 있으니 용도가 명확해서 좋지 않냐고 하고 단점이라고 하는 사람들은 걍 type이 상위호환이면 통일해서 type 쓰면 되지 뭘 인터페이스 같은 걸 씀? 이라고 하는 사람들인데 걍 상황따라 잘 맞춰쓰면 됨

전 개인적으로 intersection type이랑 아닌 거랑 구분할 때
객체 타입이냐 아니냐....를 중점적으로 이야기하는 편인데
intersection 타입 같은 경우에는 더 이상 객체라고 볼 수 없는데
따라서 keyof 같은 것도 갈길 수가 없습니다.

> 대충 type - intersection - 객체로 보기에 애매한 경우 많음 ~ 이렇게 이해함

## 추가 정리

[[Typescript] Differences Between Type Aliases and Interfaces, Type과 Interface 차이점 | Qvil Blog](https://blog.qvil.dev/typescript/differences-between-type-aliases-and-interfaces)

![[Pasted image 20230118101334.png]]

함수가 interface 에 안들어감