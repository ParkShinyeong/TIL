## 순열 & 조합

ex) [1, 2, 3]으로 만들 수 있는 경우의 수를 구하기

**순열**

순서를 부여해 조합할 수 있는 경우의 수 (중복 X)

[1, 2, 3],

**조합**

순서에 상관없이 조합 (중복 X)

**중복 순열**

중복 가능하고, 순서를 부여해 조합할 수 있는 경우의 수

## 1. 중복 순열

```jsx
// TODO: [1, 2, 3]으로 만들 수 있는 경우를 모두 구하시오 - 중복 가능/ 순서를 부여
// => 중복 순열
let arr = [1, 2, 3];

// ! 1. 반복문으로 구현하기
function OverlappingPermutation(arr) {
 let result = [];

 for (let i = 0; i < arr.length; i++) {
  for (let j = 0; j < arr.length; j++) {
   for (let k = 0; k < arr.length; k++) {
    result.push([arr[i], arr[j], arr[k]]);
   }
  }
 }
 return result;
}

let output = OverlappingPermutation(arr);
console.log(output);
console.log(output.length);

// TODO: [1, 2, 3]에서 2개만 뽑아서 만들 수 있는 경우를 모두 구하시오 - 반복문으로 구현
// 2개만 뽑으면 되니까 2중 반복문을 사용한다.
// 만약 5개 중 3개를 뽑으면 - 3중 반복문 사용
function twoOfPermutation(arr) {
 let result = [];
 for (let i = 0; i < arr.length; i++) {
  for (let j = 0; j < arr.length; j++) {
   result.push([arr[i], arr[j]]);
  }
 }
 return result;
}

let output2 = twoOfPermutation(arr);
console.log(output2);
console.log(output2.length);

// ? 그런데 만약 100개의 요소 중 50개를 뽑아서 만들 수 있는 경우를 구한다면..? 50중 반복문을 써야할까? - for문 hell...
// ? 재귀함수를 사용하여 중복 순열을 구현해보자! (시간복잡도는 별 차이없지만, 계속해서 같은 동작이 반복되므로 재귀를 사용하면 보기에도 쓰기에도 편하다.)
// TODO: [1, 2, 3]에서 2개만 뽑아서 만들 수 있는 경우를 모두 구하시오 - 재귀함수로 구현
function recursivePermutation(arr) {
 let result = [];

 // 재귀함수
 const recursive = function (count, bucket) {
  // basecase -> count는 몇개를 뽑을 건지를 의미한다. count가 0이 되면 재귀함수가 멈춘다.
  if (count === 0) {
   // butcket = 일화용 bucket, 요소 중 2개를 뽑은 것을 줍줍해서, 재귀가 끝나면 result에 push 해준다.
   result.push(bucket);
   return;
  }

  for (let i = 0; i < arr.length; i++) {
   let current = arr[i];
   recursive(count - 1, bucket.concat(current)); // ? 여기서 왜 bucket.push는 안될까?
  }
 };

 recursive(2, []);
 return result;
}

let output3 = recursivePermutation(arr);
console.log(output3);
console.log(output3.length);

// ? 재귀 (반복문 {재귀함수}) => 1번 실행하면  반복문 { 반복문 } 즉 2중 반복문
// ? 재귀함수가 실행되면, 실행된 만큼 ~중 반복문이라고 볼 수 있다.

// ? bucket.push가 안되는 이유 =>
// arr.push는 arr를 리턴하는 것이 아닌 arr.length를 리턴하기 때문이다.
```

## 2. 순열

```jsx
// TODO: 순열은 순서를 부여해 조합할 수 있는 경우의 수, 중복 X
// ? 즉 [1, 2, 3], [1, 3, 2], [2, 1, 3] ... 같은 수로 이루어져 있어도 순서가 다르므로 다른 경우로 본다.
// ? 중복 순열에서 중복을 걸러주면 된다.

let arr = [1, 2, 3];

// TODO: 1, 2, 3으로 만들 수 있는 순열 경우를 모두 구하시오 - 반복문으로 구현
function Permutation(arr) {
 let result = [];

 for (let i = 0; i < arr.length; i++) {
  for (let j = 0; j < arr.length; j++) {
   for (let k = 0; k < arr.length; k++) {
    // ? 중복된 경우를 빼야하므로, i !== j !== k 일 때만, result에 push 해준다.
    if (i !== j && j !== k && i !== k) {
     result.push([arr[i], arr[j], arr[k]]);
    }
   }
  }
 }
 return result;
}

let output = Permutation(arr);
console.log(output);
console.log(output.length);

// TODO: [1, 2, 3]에서 2개만 뽑아서 만들 수 있는 경우를 모두 구하시오 - 반복문으로 구현
function twoOfPermutation(arr) {
 let result = [];
 for (let i = 0; i < arr.length; i++) {
  for (let j = 0; j < arr.length; j++) {
   // ? 중복된 경우를 빼기 위해 걸러준다.
   if (i !== j) {
    result.push([arr[i], arr[j]]);
   }
  }
 }
 return result;
}

let output2 = twoOfPermutation(arr);
console.log(output2);
console.log(output2.length);

// TODO: [1, 2, 3]에서 2개만 뽑아서 만들 수 있는 경우를 모두 구하시오 - 재귀함수로 구현
function recursivePermutation(arr) {
 let result = [];

 // 재귀함수
 const recursive = function (count, bucket) {
  // basecase -> count는 몇개를 뽑을 건지를 의미한다. count가 0이 되면 재귀함수가 멈춘다.
  if (count === 0) {
   // butcket = 일화용 bucket, 요소 중 2개를 뽑은 것을 줍줍해서, 재귀가 끝나면 result에 push 해준다.
   result.push(bucket);
   return;
  }

  for (let i = 0; i < arr.length; i++) {
   let current = arr[i];
   // ? 중복된 수가 들어가면 안되므로, bucket에 수가 들어가있지 않을 때 재귀함수를 실행한다.
   if (!bucket.includes(current)) {
    recursive(count - 1, bucket.concat(current));
   }
  }
 };

 recursive(2, []);
 return result;
}

let output3 = recursivePermutation(arr);
console.log(output3);
console.log(output3.length);
```

## 3. 조합

```jsx
// TODO: 조합은 순서에 상관없이 조합할 수 있는 경우의 수이다. (순서 X, 중복 X)

let arr = [1, 2, 3, 4];

// TODO: 1, 2, 3, 4 중 3개만 뽑아 만들 수 있는 조합 경우를 모두 구하시오 - 반복문으로 구현
function Combination(arr) {
 let result = [];
 for (let i = 0; i < arr.length; i++) {
  for (let j = i + 1; j < arr.length; j++) {
   for (let k = j + 1; k < arr.length; k++) {
    if (i !== j || j !== k || i !== k) {
     result.push([arr[i], arr[j], arr[k]]);
    }
   }
  }
 }
 return result;
}

let output = Combination(arr);
console.log(output);
console.log(output.length);
```

---

## 최대 공약수 (GCD)

둘 이상의 공약수 중 최대인 수

**유클리드 호제법 (Euclidean-algorithm)**

⇒ \*\*\*\*최대 공약수를 구하는 알고리즘

**호제법** ⇒ 두 수가 서로 상대방 수를 나누어서 결국 원하는 수를 얻는 알고리즘

유클리드 호제법을 사용하려면 MOD 연산에 대해 알고 있어야 한다.

<aside>
❓ MOD 연산 : 두 값을 나눈 나머지를 구하는 연산

</aside>

ex) 1112와 695의 최대공약수를 구한다.

1. 큰 수를 작은 수로 나눈 나머지를 구한다.

```jsx
1112 mod 695 = 417
```

1. 그 다음 나눴던 수와 나머지로 또 MOD 연산을 한다.

```jsx
695 mod 417 = 278
```

1. 이 과정을 계속 반복한다.

```jsx
417 mod 278 = 139
278 mod 139 = 0
```

이렇게 하다가 나머지가 0이 되었을 때, 나누는 수로 사용된 139가 1112와 695의 최대공약수가 된다!

이를 JS 코드로 구현하면 다음과 같다.

```jsx
function GCD(m, n) {
 if (m % n === 0) return n;
 return GCD(n, m % n);
}
```

[참고](https://velog.io/@yerin4847/W1-%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C-%ED%98%B8%EC%A0%9C%EB%B2%95) / [관련 문제](https://programmers.co.kr/learn/courses/30/lessons/12953)
