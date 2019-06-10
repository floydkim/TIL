---
title: 2019-05-14 Daily Algorithm
date: 2019-05-14 20:57:21
category: TIL
---
2019-05-14

## Codility
A 배열은 다음 엘리먼트들로 이뤄져있다: 짝을 이루는 숫자들, 짝을 이루지 않는 하나의 숫자.

짝을 이루지 않는 숫자를 리턴한다.

```js
function solution(A) {
    // 1. 엘리먼트의 값을 key로, 각 값에 해당하는 엘리먼트 개수를 센다.
    // 1-1. 셀때 unpaired 후보 오브젝트에 등록한다.
    // 1-2. 이미 있다면 후보 오브젝트에서 지운다.
    // 2. 반복이 끝나면, 유일한 object의 키를 리턴한다.
    
    const dict = {};
    const nominee = {};
    A.forEach(cur => {
        if (!dict[cur] || dict[cur] % 2 === 0) {
            dict[cur] = 1;
            nominee[cur] = cur;
        } else {
            dict[cur] += 1;
            delete nominee[cur];
        }
    });
    
    const keys = Object.keys(nominee);
    if (keys.length === 0) {
        return 0;
    }
    return Number(keys[0])
}
```

깔끔하게 풀지는 못한거같다. 테스트 케이스를 모두 알 수 없어서, 제출 후 결과로 어떤부분이 문제인지 유추해야 했다.

문제 설명에는 하나의 엘리먼트를 제외하고 각 쌍이 유니크한 값으로 짝을 이루는 것 처럼 써놨는데, 테스트 케이스에는 [1, 1, 1, 2, 2] 처럼, 홀수개 존재하는 경우도 포함된다.

그 경우에 대응하기 위해서 dict에 숫자를 센게 이미 짝수(사실 2인 경우만 써도 작동할거다)개 존재하는 경우에 nominee에 다시 그 수를 등록하도록 했다.

어쨌든 O(n) 수준으로 해결했다..

> Detected time complexity: O(N) or O(N*log(N))
