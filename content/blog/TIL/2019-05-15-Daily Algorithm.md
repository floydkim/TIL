---
title: Daily Algorithm
date: 2019-05-15 20:57:21
category: TIL
---
2019-05-15

## Codility
A 어레이가 주어지고, K 정수 만큼 오른쪽으로 어레이 내용을 미루는 문제. (rotate 시키는 문제)

```js
function solution(A, K) {
    // boundary condition 적용
    // K만큼 미룬 인덱스를 길이로 나눈 나머지가 다시 어레이의 앞쪽으로 가리킬 것이므로
    // 자연스럽게 loop이 형성된다.
    
    // 길이만큼 미뤄서 변화가 없어야 할 경우 바로 원본 어레이 리턴
    if (A.length / K === 1) {
        return A;
    }
    
    let resultArr = [];
    for (let i = 0; i < A.length; i++) {
        resultArr[(i + K) % A.length] = A[i];
    }
    return resultArr;
}
```
pop과 unshift를 이용해 하나씩 옮겨줄 수도 있겠지만, 이전에 물리 계산 C 프로그램 짤 때 boundary condition을 만들던것 생각나서 array의 length라는 boundary를 벗어날 때 array의 0번 인덱스쪽으로 연결하는 방식으로 구현했다.

처음에는 resultArr[i] = A[(i + K) % A.length] 이렇게 할당해서 제대로 동작하지 않았는데, 그림그리면서 곰곰히 생각해보니 할당을 잘못하고있음을 깨달았다. 고친 코드로 테스트를 무사히 통과했다.
