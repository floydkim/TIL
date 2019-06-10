# async - await 에 대해..
실행 순서를 보장할 수 없는 비동기 요청을 처리하는 함수에 대해 순차적인 처리를 하기 위해서는
요청에 응답이 온 후 콜백을 실행하도록 만들고,
그 콜백에 다음에 올 처리를 맡기면 된다.

그런데 순차 실행 단계가 3단계 이상.. 많아지면 콜백 지옥에 빠지게 된다.

Promise를 사용하면 콜백 지옥 대신 .then() 체이닝을 이용해 덜 복잡하게 표현할 수 있다.

await은 Promise를 위한 문법적 sugar다. 사람이 코드를 다루기 좋게 하는 목적임.
async function 선언한 함수 안에서 await을 사용하면, 비동기적인 작업에 순서를 지키도록 해주고, 따라서 동기적이게 된다. 실행 순서를 보장해준다.
```javascript
function printString(string){  
  return new Promise((resolve, reject) => setTimeout(  
    () => {  
      console.log(string)  
      resolve()  
    },   
    Math.floor(Math.random() * 1000) + 1 )  
  )
}

async function printAll() {
  await printString("A");
  await printString("B");
  await printString("C");
}
printAll();
```
위 코드는 printString함수가 setTimeout을 이용해 비동기적으로 랜덤한 시간이 지난 후 콘솔 출력을 함에도 불구하고
콘솔에 A, B, C 순서대로 찍히도록 보장해준다.
각 await 라인의 함수가 실행이 끝날때까지 기다리고, Promise가 fulfill되면 다음 라인으로 간다. (async함수의 실행을 멈춰서 기다린다.)

async함수는 Promise를 리턴한다.

아래는 이해했는지 확인하기 위해 대강 콘솔에 적은 코드다.
확실히 `await`은 Promise가 응답하길 기다리며 그 라인에 멈춘다. 
```javascript
async function afunc() {
  console.log("START")
  const res = await (function(){
    return new Promise((resolve, reject) => {
    setTimeout(()=>{console.log("ASDF");resolve("resolved!");}, 2000);
  }
  )})();
  console.log("END", res)
}
afunc();
```
[이해에 도움을 준 글](https://medium.com/front-end-weekly/callbacks-promises-and-async-await-ad4756e01d90)

### 나중에 추가한 내용
[다른 좋은 참고자료](https://javascript.info/async-await)
> The word “async” before a function means one simple thing: a function always returns a promise. Even If a function actually returns a non-promise value, prepending the function definition with the “async” keyword directs Javascript to automatically wrap that value in a resolved promise.

그리고 async function 안에서 non-Promise 값을 리턴하면 자동으로 그걸 resolved Promise로 감싸 값으로 담아준다.
(명시적으로 Promise를 리턴해주면 그냥 그대로 리턴해준다)

**Error Handling**
일반적으로 `await` 으로 Promise의 fulfilled를 기다려서 결과를 받으면 그 Promise가 rejected 상태로 끝난다면 `catch()`를 이용해 자연스럽게  에러 핸들링이 가능하다.

하지만 지금은 async function 자체의 특성을 알아보고자 하므로,  `catch`로 잡아낼 수 있는 에러는 어떻게 만들어낼 수 있는지 알아보았다.

아래와 같이 가능하다.
```javascript
var af = async function() {
  throw new Error("error occurred!!!!!");
}
```
`throw`로 에러를 발생시키면 `catch`로 잡아줄 수 있다.
```javascript
af().then(r=>{
  console.log(r);
  return r;
}).catch(e=>{
  console.log(e);
  return e+"ERRERR";
});

// 콘솔에 e의 내용을 띄우고
//Error: error occurred!!!!!
//    at af (<anonymous>:2:9)
//    at <anonymous>:1:1

// return e+"ERRERR"; 때문에 아래 Promise를 리턴한다
// Promise {<resolved>: "Error: error occurred!!!!!ERRERR"}
```
`then()`을 건너뛰고 `catch()`에 의해 변경된 내용을 확인할 수 있다.

[링크의 글(error handling section)](https://javascript.info/async-await#error-handling)을 보면 try - catch 구문을 이용한 핸들링 예제도 있다.

