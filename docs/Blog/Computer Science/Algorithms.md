---  
category: Blog/Computer Science  
share: true  
title: Algorithm  
---  
# Binary Search  
- [금과 은 운반하기](https://school.programmers.co.kr/learn/courses/30/lessons/86053#)  
  
  
## 풀이  
  
```js  
const solution = (  
  requiredGold,  
  requiredSilver,  
  nearByGolds,  
  nearBySilvers,  
  nearByweights,  
  nearByTime  
) => {  
  // 이분탐색의 범위: 최대 걸리는 시간  
  // 편도 최대시간 t = 10e5 -> 왕복 시 t = 10e5 * 2  
  // 한번에 1kg 씩 옮긴다 가정할 경우 금, 은을 모두 옮기는데 걸리는 총 시간  
  // t = (10e5 * 2) * (2 * 10e9)  
  let time = 10e5 * 4 * 10e9;  
  let start = 0;  
  let end = 10e5 * 4 * 10e9;  
  while (start <= end) {  
    let mid = Math.floor((start + end) / 2);  
    let goldCarry = 0;  
    let silverCarry = 0;  
    let allCarry = 0;  
  
    for (let i = 0; i < nearByGolds.length; i++) {  
      const goldReserves = nearByGolds[i]; // i국 금 (kg gold/city)  
      const silverReserves = nearBySilvers[i]; // i국 은 (kg silver/city)  
      const allowableWeight = nearByweights[i]; // i국에서 한번에 옮길 수 있는 최대 양 (kg/city)  
      const oneWayTime = nearByTime[i]; // i국에서 한번 옮길 때 걸리는 편도 시간 (hr/city)  
      // 도시 → 목적지 경로 횟수  
      const totalDeliveryCount = Math.ceil(Math.floor(mid / oneWayTime) / 2);  
      // 해당 도시가 최대로 옮길 수 있는 광물의 양 (kg): 전달 횟수 * 한번에 옮길 수 있는 양  
      const totalMineralConveyCapacity = totalDeliveryCount * allowableWeight;  
  
      // 금만 캤을 때 캘 수 있는 양  
      goldCarry += Math.min(totalMineralConveyCapacity, goldReserves);  
      // 은만 캤을 때 캘 수 있는 양  
      silverCarry += Math.min(totalMineralConveyCapacity, silverReserves);  
      // 최대로 옮길 수 있는 광물의 무게  
      allCarry += Math.min(  
        totalMineralConveyCapacity,  
        silverReserves + goldReserves  
      );  
    }  
    // 시간(mid)안에 가능한 경우  
    if (  
      requiredGold <= goldCarry &&  
      requiredSilver <= silverCarry &&  
      requiredGold + requiredSilver <= allCarry  
    ) {  
      end = mid - 1;  
      time = Math.min(time, mid);  
    } else {  
      // 이 시간안에 해결 불가한 경우  
      start = mid + 1;  
    }  
  }  
  return time;  
};  
  
```  
  
  
  
먼저 이 문제를 결정문제로 환원해보자.   
시간  $T$ 가 주어졌을 때,  $T$ 라는 시간 안에 트럭을 최대한 활용시켜 금과 은을 각각  $a,  b$ 만큼 운반할 수 있는지 판별하는 문제로 바꾸어 접근하면 된다.  
  
그렇다면 해당 결정 문제는 다음과 같이 풀 수 있다.  
  
1. 제한 시간 안에 금을 최우선적으로 운반했을 때 운반할 수 있는 금의 양과 은의 양을 각각 $G_{max}, S_{min}$  정의 (금 우선 탐색)  
2. 제한 시간 안에 은을 최우선적으로 운반했을 때 운반할 수 있는 금의 양과 은의 양을 각각 $G_{min}, S_{max}$  정의 (은 우선 탐색)  
3. $G_{max} + S_{min}$   = $G_{min}, S_{max}$  
4. 만약 다음과 같은 조건을 만족할 경우 주어진 시간 $T$ 안에 a 만큼 금과 b 만큼 은을 운반할 수 있다.  
	- $a \leq G_{max}$  
	- $b \leq S_{max}$  
	- $a + b \leq G_{max} + S_{min} = G_{min} + S_{max}$  
  
특정시간 $T$ 에서   
  
- `goldCarry` = 특정시간 $T$ 동안 얻을 수 있는 최대 골드 수 ($Gmax$)  
  
- `silverCarry` = 특정시간 $T$  동안 얻을 수 있는 최대 실버 수 ($Smax$)  
  
- `allCarry` = 특정시간 $T$  동안 골드와 실버를 한번에 얻을 수 있는 최대 수 ($Gmin + Smax$)  
  
  
이 세가지 값을 가지고 $a + b <= Gmax + Smin = Gmin + Smax$ 이조건을 만족한다면 현재 t시간은 a, b를 운반할 수 있다를 결정할 수 있게된다.  
  
  
  
  
- 4번 성립하는 이유 증명  
![Pasted image 20230116121553.png](../../attachments/Pasted%20image%2020230116121553.png#)  
  
  
  
> [!hint]- 참고 문서   
> - [[월간 코드 챌린지 시즌3] 9월 문제 해설 :: 프로그래머스 공식 블로그](https://prgms.tistory.com/101)  
> - [프로그래머스 : 금과 은 운반하기](https://redbinalgorithm.tistory.com/696)  
> - [[ 프로그래머스 [ 월간코드챌린지 ] 금과 은 운반하기 ] (C++) :: 얍문's Coding World..](https://yabmoons.tistory.com/714)  
  
