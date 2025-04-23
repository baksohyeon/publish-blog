---
category: 질문 목록/Solved Problems/Error
share: true
title: JS 다중 배열 객체 선언
---
  
# JS 다중 배열 객체 선언  
  
## 질문 내용  
  
```js  
const solution = (n, left, right) => {  
  const matrix = [...Array(n)].fill([...Array(n).keys()].map((x) => x + 1));  
  
  matrix[2][2] = 10;  
  return matrix;  
};  
  
console.log(solution(3, 2, 5));  
  
// matrix : [ [ 1, 2, 3 ], [ 1, 2, 3 ], [ 1, 2, 3 ] ]  
//                0             1            2  
  
// 예상 결과: [ [ 1, 2, 3 ], [ 1, 2, 3 ], [ 1, 10, 3 ] ]  
// 실제 결과: [ [ 1, 2, 10 ], [ 1, 2, 10 ], [ 1, 2, 10 ] ]  
```  
  
## 질문 관련 서치 내용  
  
[[JavaScript] 자바스크립트 2차원 배열 선언 및 사용법](https://gent.tistory.com/296)  
  
## 질문 답변 (해결 방안)  
  
```js  
const solution = (n, left, right) => {  
  const array = [...Array(n).keys()].map((x) => x + 1);  
  const matrix = [...Array(n)];  
  for (let i = 0; i < matrix.length; i++) {  
    matrix[i] = array;  
  }  
  
  matrix[2][2] = 10;  
  return matrix;  
};  
  
console.log(solution(3, 2, 5));  
```  
  
이러면 matrix 안에 들어 있는 세 배열이 전부 똑같은 배열임  
그래서 하나 바꾸면 다 바뀜  
  
matrix에 들어 있는 세 배열이 전부 같은 배열임  
  
---  
