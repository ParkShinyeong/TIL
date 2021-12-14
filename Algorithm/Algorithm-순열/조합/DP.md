## DP (Dynamic Programming, 동적 계획법)

모든 경우의 수를 조합해 최적의 해법을 찾는 방식

(탐욕 알고리즘과 같이 작은 문제에서 출발한다는 점은 같으나, 탐욕 알고리즘은 매 순간 최적의 선택을 찾는 방식이라는 점에서 DP와 다르다.)

- 주어진 문제를 여러 개의 하위 문제로 나누어 풀고, 하위 문제들의 해결 방법을 결합하여 최종 문제를 해결하는 문제해결 방식
- 동일한 하위문제를 만날 시 저장된 해결책을 적용해 계산 횟수를 줄인다.(memoization)
#
**DP는 다음 두 조건에서 사용할 수 있다.**

- 큰 문제를 작은 문제로 나눌 수 있고, 이 작은 문제가 **중복**해서 발견된다. ⇒ 부분 문제의 반복 (Overlapping Subproblems)
  ```json
  //만약 피보나치 4를 구하려고 하면 ..
  fibonacci(0) = 0;
  fibonacci(1) = 1;
  fibonacci(2) = fibonacci(1) + fibonacci(0);
  fibonacci(3) = fibonacci(2) + fibonacci(1) = fibonacci(1) + fibonacci(1) + fibonacci(0);
  fibonacci(4) = fibonacci(3) + fibonacci(2) = fibonacci(2) + fibonacci(1) + fibonacci(2) = ...;
  //중복적으로 실행되는 함수가 많음을 알 수 있다.
  ```
- 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 같다. 즉 작은 문제에서 구한 정답을 큰 문제를 해결할 때 사용할 수 있다. ⇒ 최적 부분구조 (Optimal Substructure)
#
### Memoization을 활용한 피보나치 수열

그냥 재귀함수를 이용해 피보나치 수열을 풀었을 때 ⇒

```jsx
function fibonacci(n) {
 if (n <= 1) {
  return n;
 }
 return fibonacci(n - 1) + fibonacci(n - 2);
}
```

그런데 이 경우에는 **함수가 중복적으로 실행**되는 경우가 많다.

```jsx
//만약 피보나치 4를 구하려고 하면 ..
fibonacci(0) = 0;
fibonacci(1) = 1;
fibonacci(2) = fibonacci(1) + fibonacci(0);
fibonacci(3) = fibonacci(2) + fibonacci(1) = fibonacci(1) + fibonacci(1) + fibonacci(0);
fibonacci(4) = fibonacci(3) + fibonacci(2) = fibonacci(2) + fibonacci(1) + fibonacci(2) = ...;
//중복적으로 실행되는 함수가 많음을 알 수 있다.
```

이렇게 중복적으로 실행되는 경우가 많은 경우, n이 커질 때 점점 실행 속도가 늘어나면서, 한참을 기다려야 할 수도 있다. ⇒ 시간 복잡도 ⇒ O(2^N)

더 효율적으로 작성하려면 어떻게 해야할까?
#
### ⇒ memoization : O(N)

이미 해결한 문제의 정답을 따로 기록해두고, 다시 해결하지 않는 기법을 이야기한다.

- 정답을 한번 구했으면 그 정답을 **캐시에 메모**해놓는다. 이렇게 메모하는 것을 코드로 구현 ⇒ 배열에 값을 저장
- 모든 문제를 한번씩만 푼다. 따라서 **시간 복잡도**는 `문제의 개수 * 문제 1개를 푸는 시간` → 문제의 개수가 N개일 때 전체 시간 복잡도는 **O(N)**이다.

```jsx
function fibonacci(n) {
 // 중복적인 호출을 줄일 수 있도록 한번 계산한 값을 저장하여 사용한다.

 let result = [0, 1]; // result가 캐시 역할을 한다.

 function memo(n) {
  // result[n]이 undefined가 아니면 이미 값이 존재하므로, 함수가 실행되었음을 알 수 있다.
  if (result[n] !== undefined) {
   return result[n]; // 저장한 값을 꺼내서 사용한다.
  }
  // result[n]이 undefined이면 아직 함수가 실행되지 않음을 알 수 있다.
  // result[n]에 함수의 실행 값을 저장해준다.
  result[n] = memo(n - 1) + memo(n - 2);
  return result[n];
 }
 return memo(n);
}
```

자세한 내용은 [여기](https://velog.io/@polynomeer/%EB%8F%99%EC%A0%81-%EA%B3%84%ED%9A%8D%EB%B2%95Dynamic-Programming)를 참고하자! (동적 계획법)
