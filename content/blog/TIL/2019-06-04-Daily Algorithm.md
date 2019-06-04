---
title: Daily Algorithm
date: 2019-06-04 17:06:23
category: TIL
---
2019-06-04
## Programmers
### 폰켓몬

nums 배열에 폰켓몬 종류에 대응하는 숫자가 담겨있다.
> [3, 2, 1, 3]

nums에 담겨 온 폰켓몬 N개 중 N/2개 가져가는데, 최대한 많은 종류를 골라 가져간다.
이 때 폰켓몬 종류 수를 리턴하는 함수를 작성하는 문제.

> -- nums의 길이(N)는 1 이상 10,000 이하의 자연수이며, 항상 짝수로 주어집니다.
-- 폰켓몬의 종류 번호는 1 이상 200,000 이하의 자연수로 나타냅니다.

```javascript
function solution(nums) {
    // input [3,2,1,3] output 2
    // 1. 엘리먼트의 종류:갯수 를 오브젝트에 기록
    // 2. 종류의 수가 length/2 보다 크거나 같으면 length/2 가 최대값
    // 3. 종류의 수가 length/2 보다 작으면 종류의 수가 최대값
    var answer = 0;
    const dictionary = {};
    // let numberOfMons = 0; // answer로 대체
    for (let i = 0; i < nums.length; i++) {
        if (!dictionary[nums[i]]) {
            dictionary[nums[i]] = 1;
            answer += 1;
        }
    }
    
    // const numberOfMons = Object.keys(dictionary).length; // answer로 대체

    // return 시 삼항연산자로 대체
    // if (numberOfMons >= nums.length / 2) {
    //     answer = nums.length / 2;
    // } else {
    //     answer = numberOfMons;
    // }
    
    const halfOfNumslength = nums.length / 2; // 변수를 하나 늘리지만 나누기 연산을 한번만 하기 위함
    return answer >= halfOfNumslength ? halfOfNumslength : answer;
}
```

스터디하는 날 페어 코딩 인터뷰 문제로 받아 풀었음.

Time Complexity O(n).

배열에 담긴 엘리먼트들의 종류 수를 세려면 모든 엘리먼트를 순회한다. 중복 체크를 위해 dictionary 오브젝트를 생성한다.

처음엔 몇 개 있는지도 세었으나 삭제해 연산을 줄였고, answer를 이용하는것도 문제의 일부라고 생각해 answer 변수에 종류 수를 세었다.

종류 수가 아무리 많아도 가져갈 수 있는 최대 폰켓몬 수는 N/2개 이므로, 종류 수가 N/2보다 같거나 크면 N/2를 리턴한다.

종류 수가 N/2보다 적으면 가져갈 수 있는 폰켓몬 수(N/2)가 아무리 커도 세어놓은 종류 수를 넘기지 못하므로 종류 수를 리턴한다.
