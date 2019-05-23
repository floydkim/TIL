---
title: Daily Algorithm
date: 2019-05-23 20:57:21
category: TIL
---
2019-05-23
## Programmers
### 정수 내림차순으로 배치하기
임의의 여러자리 정수가 주어졌을 때, 각 자리의 숫자를 내림차순으로 재 배치한 숫자를 리턴하는 함수를 작성한다.
```js
function solution(n) {
    var arr = String(n).split("");
    var sorted = arr.sort((a, b) => b - a);
    
    return Number(sorted.join(""));
}
```
쉬운 문제다. Complexity를 개선하려고 파본다면 어떤 방법이 나올지 모르겠다.

1. Array.prototype.sort 메서드를 사용하기 위해서, 들어온 숫자를 String으로 바꿔 split해 array로 만들었다.
2. 내림차순으로 sort
3. join해 string으로 만들고, Number로 형변환 해 리턴

### 시저 암호
주어진 string의 각 문자열을 주어진 n만큼 밀어 변형한 고전적인 암호화 방법인 시저 암호를 구현하는 문제.
- 입출력 예시
AB , 1 -> BC
a B , 2 -> c D
Z , 1 -> A
z , 1 -> a
(공백은 변형하지 않음)
```js
function solution(s, n) {
    // string 전체를 순회하는데 각 인덱스 즉 각 문자열의 charCode를 얻어 n을 더한 charCode를 문자열로 변환해 결과에 concat한다
    // 이 때 문자열이 공백이라면 n을 더하지 않는다
    // z에 해당하는 charCode를 벗어나면 다시 a부터 시작하도록 경계조건을 걸어준다. 단, 대문자는 대문자끼리, 소문자는 소문자끼리 loop을 만든다.
    // a~z : 97 ~ 122  //  A~Z : 65 ~ 90
    const alphabet = "abcdefghijklmnopqrstuvwxyz";
    const ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    let result = "";
    for (let i = 0; i < s.length; i++) {
        let charCode = s[i].charCodeAt();
        if (s[i] === " ") {
            // 공백일 경우
            result += " ";
            continue;
        }
        if (charCode > 96) {
            // 소문자일 경우
            result += alphabet[(charCode + n - 97) % 26];
        } else {
            // 대문자일 경우
            result += ALPHABET[(charCode + n - 65) % 26];
        }
    }
    return result;
}
```
알고리즘은 코드에 써둠. Time Complexity는 O(n). 각 문자열마다 constant한 연산을 한다.
z에서 a로 다시 넘어오는 loop이 발생하는 경우를 나머지 연산자로 처리하고 나니 0~25의 인덱스 형태가 리턴되어 이것을 약간의 인력을 들여 알파벳 배열을 둘 만들고, 배열의 인덱스로부터 문자열으 가져오는것이 연산이 빠르다 판단했음.
