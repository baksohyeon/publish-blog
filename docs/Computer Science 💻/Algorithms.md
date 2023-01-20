---  
category: Computer Science 💻  
share: true  
title: Algorithms  
date created: Monday, January 16th 2023, 3:20:49 pm  
date modified: Wednesday, January 18th 2023, 5:01:30 pm  
---  
# Binary Search  
- [금과 은 운반하기](https://school.programmers.co.kr/learn/courses/30/lessons/86053#)  
  
먼저 이 문제를 결정문제로 환원해봅시다. 시간  $T$ 가 주어졌을 때,  $T$ 라는 시간 안에 트럭을 최대한 활용시켜 금과 은을 각각   
$a,  b$ 만큼 운반할 수 있는지 판별하는 문제로 바꾸어 접근하면 된다.  
  
그렇다면 해당 결정 문제는 다음과 같이 풀 수 있다.  
  
1. 제한 시간 안에 금을 최우선적으로 운반했을 때 운반할 수 있는 금의 양과 은의 양을 각각 $G_{max}, S_{min}$  정의 (금 우선 탐색)  
2. 제한 시간 안에 은을 최우선적으로 운반했을 때 운반할 수 있는 금의 양과 은의 양을 각각 $G_{min}, S_{max}$  정의 (은 우선 탐색)  
3. $G_{max} + S_{min}$   = $G_{min}, S_{max}$  
4. 만약 다음과 같은 조건을 만족할 경우 주어진 시간 $T$ 안에 a 만큼 금과 b 만큼 은을 운반할 수 있다.  
	- $a \leq G_{max}$  
	- $b \leq S_{max}$  
	- $a + b \leq G_{max} + S_{min} = G_{min} + S_{max}$  
  
특정시간 $T$ 에서   
  
   
  
gold = 특정시간 $T$ 동안 얻을 수 있는 최대 골드 수  
  
silver = 특정시간 $T$  동안 얻을 수 있는 최대 실버 수  
  
add = 특정시간 $T$  동안 골드와 실버를 한번에 얻을 수 잇는 최대 수  
  
   
  
이 세가지 값을 가지고 $a + b <= Gmax + Smin = Gmin + Smax = add$ 이조건을 만족한다면 현재 t시간은 a, b를 운반할 수 있다를 결정할 수 있게된다.  
  
  
  
  
- 4번 성립하는 이유 증명  
![Pasted image 20230116121553.png](../assets/img/Pasted%20image%2020230116121553.png)  
  
  
  
> [!hint]- 참고 문서   
> - [[월간 코드 챌린지 시즌3] 9월 문제 해설 :: 프로그래머스 공식 블로그](https://prgms.tistory.com/101)  
> - [프로그래머스 : 금과 은 운반하기](https://redbinalgorithm.tistory.com/696)  
> - [[ 프로그래머스 [ 월간코드챌린지 ] 금과 은 운반하기 ] (C++) :: 얍문's Coding World..](https://yabmoons.tistory.com/714)  
  
  
  
```js  
function solution(a, b, g, s, w, t) {  
    let answer = 10e5 * 4 * 10e9;  
      
    let start = 0;  
    let end = 10e5 * 4 * 10e9;  
      
    while( start <= end) {  
        let mid = Math.floor((start + end) / 2);  
        let gold = 0;  
        let silver = 0;  
        let add = 0;  
          
        for( let i =0; i < s.length; i++ ) {  
            let now_g = g[i];  
            let now_s = s[i];  
            let now_w = w[i];  
            let now_t = t[i];  
              
            let move_cnt = Math.floor(mid / (now_t * 2));  
            if(mid % (now_t * 2) >= t[i]) move_cnt++;  
  
            gold += (now_g < move_cnt * now_w) ? now_g : move_cnt * now_w;  
            silver += (now_s < move_cnt * now_w) ? now_s : move_cnt * now_w;  
            add += (now_g + now_s < move_cnt * now_w) ? now_g + now_s : move_cnt * now_w;  
        }  
          
          
        if(gold >= a && silver >= b && add >= a + b) {  
            end = mid - 1;  
            answer = Math.min(mid, answer);  
        }else {  
            start = mid + 1;  
        }  
    }  
      
    return answer;  
}  
```