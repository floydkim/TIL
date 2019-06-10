---
title: 2019-05-13 Daily Algorithm
date: 2019-05-13 20:57:21
category: TIL
---
2019-05-13

## Haker rank
- `ar` : 양말의 색을 숫자로 표현한 어레이
만약 [10, 20, 20, 10, 10, 30]이 주어졌다면, 같은 색 양말이 10 한 쌍, 20 한 쌍으로 총 두 쌍 존재하며 그 총 수를 리턴하는 문제.
- `n`: 총 양말 수. 왜 준건지 모르겠다.

```javascript
function sockMerchant(n, ar) {
    let dic = {};
    let count = 0;
    ar.forEach(cur => {
        if (!dic[cur]) {
            dic[cur] = 1;
        } else {
            dic[cur] += 1;
        }
        if (dic[cur] % 2 === 0) {
            count++;
        }
    })
    return count;
}
```
처음 생각했던 알고리즘에서 좀 고쳐서 for문 한번만으로 O(n) complexity 갖도록 풀었다.

## codility
10진수 N이 주어지고 이걸 2진수로 표현했을때 1과 1 사이의 거리(binary gap)를 구한다. gap이 여럿 있다면, 가장 큰 gap을 정수로 리턴하는 문제.

(ex) 10001001 이라면, gap은 3과 2가 있는데, 3을 리턴하면 된다.

(ex) 10000 이라면, gap이 없으니 0 리턴할 것. 1111 이라면, gap은 0이니 0을 리턴할 것.

```javascript
function solution(N) {
    // write your code in JavaScript (Node.js 8.9.4)
    let binary = N.toString(2);
    // 10001001
    // 1. binary string에서 모든 1의 인덱스를 순서대로 저장한다 (어레이에)
    // 2. 각 엘리먼트간 차를 구해 가장 큰 수를 리턴한다.
    // (예외처리) 1이 없거나, 1이 하나뿐이면 0을 리턴한다. (어레이 length로 판단)
    
    let indices = [];
    
    for (let i = 0; i < binary.length; i++) {
        if (binary[i] === "1") {
            indices.push(i);
        }
    }
    
    if (indices.length === 0 || indices.length === 1) {
        return 0;
    }
    
    let largest = 0;
    for (let i = 1; i < indices.length; i++) {
        if (indices[i] - indices[i - 1] - 1 > largest) {
            largest = indices[i] - indices[i - 1] - 1;
        }
    }
    return largest;
}
```
