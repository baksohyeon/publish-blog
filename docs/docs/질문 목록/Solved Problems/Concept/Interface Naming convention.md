---  
category: 질문 목록/Solved Problems/Concept  
share: true  
title: Interface Naming convention  
---  
  
# Interface Naming convention  
  
**질문 날짜**: 2022.11.19.Saturday  
**태그**:  
**관련 글**: [4주차](../../../../../4%EC%A3%BC%EC%B0%A8.md), [의존성 주입 DI](../../../../../%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%A3%BC%EC%9E%85%20DI.md), [코드 분리와 인터페이스, 추상클래스와 인터페이스 차이](../../../../../%EC%BD%94%EB%93%9C%20%EB%B6%84%EB%A6%AC%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4,%20%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%20%EC%B0%A8%EC%9D%B4.md)  
  
---  
  
## 질문 내용  
  
객체 지향을 공부하면서 `Interface` 라는 것을 써보려고 하는데  
이름 컨벤션에 대해서 궁금  
파일 구조를 어떻게 짜야할지 고민중  
  
## 질문 관련 서치 내용  
  
### #1  
  
[자바 인터페이스 이름관련: I로 시작하는 interface? impl로 끝나는 클래스?](https://stanleykou.tistory.com/entry/%EC%9E%90%EB%B0%94-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%EC%9D%B4%EB%A6%84%EA%B4%80%EB%A0%A8-I%EB%A1%9C-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-interface-impl%EB%A1%9C-%EB%81%9D%EB%82%98%EB%8A%94-%ED%81%B4%EB%9E%98%EC%8A%A4)  
  
인터페이스를 구현했다면, 그것은 클래스입니다. 유일한 예외가 있다면 (어디에나 예외는 있으니까요) AbstractTruck 과 같은 추상화클래스의 경우입니다. 이 추상클래스를 구현한 자식클래스에서만 이 클래스를 볼 것이고, 자식클래스는 절대로 Abstract 로 캐스팅 되지 않을 것이기 때문에, 이 클래스가 추상 클래스이고 이것을 어떻게 써야 하는지에 대한 정보를 제공하기 위해 이런 이름을 쓸 수 있습니다. 그리고 AbstractTruck 보다 좀 더 나은 이름을 쓸 수 있는데, BaseTruck 이나 DefaultTruck 같은 이름을 쓰고 abstract 와 같은 예약어를 피할 수 있습니다. 하지만 추상 클래스는 절대 외부로 노출되는 인터페이스에 포함되지 않아야 하기 때문에, 이 Abstract 라는 이름을 써도 외부에서는 볼 수 없으니 문제될 것은 없을 것 같습니다. 생성자를 protected 로 선언하면 명확하게 외부생성을 차단하는 것이 도움이 될 것입니다.  
  
그리고 Impl 접미사는 좀 더 거슬리는 짓입니다. 더 심한 동어반복입니다. 인터페이스(Interface)가 아닌 모든 것은 구현(Implementation) 이고, 추상 클래스조차도 구현된 것을 일부 가지고 있을 수 있습니다. 모든 클래스 뒤에 Impl 을 넣고 싶습니까?  
  
자바 스탠다드 라이브러리를 예로 들어 보겠습니다. IList, ArrayListImpl, LinkedListImpl 이런 이름을 찾을 수 있습니까? 아닙니다. 자바 표준 라이브러리는 **List, ArrayList, LinkedList** 라고 표현합니다.  
여기 이 질문에 대한 정확한 답변 글 이 있습니다. 이런 접두어 / 접미어에 대한 웃기는 이름짓기 관례는 모두 [DRY Do not Repeat Your Self (반복하지 말라)](https://www.artima.com/articles/orthogonality-and-the-dry-principle) 원칙에 어긋나는 것이기도 합니다.  
  
만약 당신이 DTO, JDO, BEAN 같은 웃기는 동어 반복적인 접미어를 사용하고 있다면, 그것은 접미어로 넣는 대신 패키지package 명에 넣어야 할겁니다. 적절하게 이름지어진 패키지명은 "자체 문서화" 이며, 자바 내부에서는 쓰이지도 않는 이런 정말 잘못 만들어진 명명 체계를 쓰는 것은 모순적인 일입니다.  
  
만약 클래스 뒤에 Impl 이라는 접미어를 붙일 수 밖에 없는 상황이라면, Interface 를 쓰는 것 자체에 대해 다시 생각해 볼 수 있습니다. 예를 들면 **Interface 와 그것을 구현한 단 하나의 구현체가 있는 경우라면, 그것은 고유하게 특수화 된 Interface 가 아니기 때문에 아마도 Interface 를 쓰지 말아야 하는 경우일 것입니다.**  
  
(역주: 테스트를 위해 클래스를 교체해야 할 필요가 있을 때, 테스트 만을 위한 Interface를 만드는 대신 Mock 등의 현대적인 방식으로 이를 대체할 수 있을 것입니다)  
  
---  
  
### #2  
  
[Java Interfaces/Implementation naming convention - Stack Overflow](https://stackoverflow.com/questions/2814805/java-interfaces-implementation-naming-convention)  
  
The name of the interface should describe the abstract concept the interface represents. Any implementation class should have some sort of specific traits that can be used to give it a more specific name.  
  
If there is only one implementation class and you can't think of anything that makes it specific (implied by wanting to name it `-Impl`), then it looks like there is no justification to have an interface at all.  
  
---  
  
### #3  
  
I tend to follow the pseudo-conventions established by Java Core/Sun, e.g. in the Collections classes:  
  
- List - interface for the "conceptual" object  
- ArrayList - concrete implementation of interface  
- LinkedList - concrete implementation of interface  
- AbstractList - abstract "partial" implementation to assist custom implementations  
  I used to do the same thing modeling my event classes after the AWT Event/Listener/Adapter paradigm.  
  
## 질문을 정리하면서 이해한 것  
  
인터페이스 구현체가 딱 한개만 있다면 그냥 패키지에만 명시해두고 쓰면 된다는 것으로 이해함 (자바 스탠다드 라이브러리 보면 꼭 안지키지 않았느냐... 접미사 드럽게 다 넣고싶으냐, 원칙에도 어긋난다)  
아니면 자바 컨벤션에 맞추어서 무조건 `-Impl` 쓰는게 국룰은 아니다~ 로 이해  
  
## 추가 질문  
  
### 질문 1  
  
> **인터페이스를 만드는 기준**  
  
연습해보려고 구현하는 파일마다 interface로 먼저 추상화해두고 로직을 짜려고 하는데 오히려 더 안좋은 것인지  
답변 1에서는 구현체가 단 하나의 경우라면 쓰지마라하는데 사실 구체적으로 왜 저렇게 이야기를 하는지 잘 이해가 안감  
  
나중에 바꿔 끼울 수 있는? 결합도를 낮춰야겠다고 생각이 드는 클래스에서만 interface를 만들어야 한다! 라는 것인가여?  
  
## 질문 답변 (해결 방안)  
  
인터페이스를 따로 제공 안 하는 게 코드를 간결하게 만드는 건 맞음  
  
사용하는 dependency injection 프레임워크라든지 여러 가지 실용적인 이유로 인터페이스를 분리하는 원리주의 코드를 짜는 게 좋을 때도 있다고 함  
  
impl을 별도의 패키지에 넣는 건 그런 클래스가 많아지면 의미가 있겠지만  
근데 타입명이 겹칠 정도인데 패키지로만 구별하는 건 좀..  
저라면 머리 안 쓰고 Impl 써서 광명 찾을듯  
뭔가 엔터프라이즈 자바(TM)스러운 코딩 문화긴 함  
  
걍 그럼 일단 인터페이스 만들고 Impl 써보고 어쩌구 해봄  
  
경험이 중요한듯  
  
이름 정하는건 통일성만 지키면 된다.  
  
패키지가 있으니까 접미사를 쓰지 않는게 좋다는건 반반  
그 코드를 사용하는 클라이언트 입장에선 패키지는 import 문에 박혀있는거라 명확히 안보임  
  
인터페이스 적용은 결국 "다형성을 통해 변경이 용이한 구조획득"이 목표인데, 말 그대로 변경이 일어나야 의미가 있는거지.. 변경할 일이 없으면 필요없음  
  
물론 변경이 안일어날거라는건 너무 섣부른 판단일 순 있음  
  
글구 인터페이스의 또 다른 목적은, 자바가 막아둔 다중상속 때문이기도 함..  
자바8 부터는 인터페이스랑 추상클래스 사이에 경계가 모호해지기도 했고  
  
<mark class="yellow">결론: 이름은 정답 없으니까 걍 하고싶은 대로! 전체 통일성을 지켜주면 된다 ! </mark>  
  
---  
