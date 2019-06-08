---
title: 2019-06-07 Daily Algorithm
date: 2019-06-07 22:06:23
category: TIL
---
2019-06-07
## Programmers
### 같은 숫자는 싫어

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 함수 작성하는 문제

제한사항
> -- 배열 arr의 크기 : 1,000,000 이하의 자연수 
> -- 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수
```javascript
function solution(arr)
{
    // [1,1,3,3,0,1,1,5,5] -> [1, 3, 0, 1, 5]
    // 1. 배열을 전부 순회하되 temp에 현 엘리먼트를 저장
    // 2. 엘리먼트가 temp와 다를때만 answer에 push
    var answer = [];
    let temp = null;
    for (let i = 0; i < arr.length; i++) {
        if (temp !== arr[i]) {
            answer.push(arr[i]);
            temp = arr[i];
        }
    }
    return answer;
}
```
