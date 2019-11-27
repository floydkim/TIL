---
title: NHN FORWARD 2019 - Java에서 Fiber를 이용하여 동시성(concurrency) 프로그래밍 쉽게 하기
date: 2019-11-27 21:32:23
category: 후기
---

2019-11-27

# Java에서 Fiber를 이용하여 동시성(concurrency) 프로그래밍 쉽게 하기

### (NHN 안성우)

게임서버 개발

  

## concurrency 프로그래밍이 어려운 이유

-   동기화 처리가 어렵다 (멀티스레드)
-   여러 스레드들의 교통정리가 잘 안됨
-   흐름 제어가 어렵다
-   비동기 함수 호출을 많이 하느라 콜백 헬이 발생하며, Promise같은걸 이용해도 어렵다.

  

## 쉽게 하는 방법

싱글 event loop로 구현 (node.js 처럼)

  

## 동기식 동시성 프로그래밍이 불가능한 이유

무거운 작업은 별도의 스레드풀에 넘기는데, 동기식인 경우 성능 문제가 발생한다.

  

## Fiber란 무엇인가?

user space thread, lightweight thread라고 할 수 있다.

스레드처럼 주소공간을 점유하여 동작.

  

## Fiber와 Thread 비교

fiber는 명시적으로 끝내고 다른쪽으로 넘겨야(yield) 넘어감. (스레드는 스케쥴러에 의해 강제로 다른 스레드로 전환)

  

  
Fiber가 메모리 점유가 훨~씬 적음.

  

## fiber는 어디에 사용하면 좋은가?

스레드 서버와 다르게 fiber는 수십만개 만들어서 많은 요청을 동시처리 가능하다

##   Coroutine이란 무엇인가?

-   루틴의 로컬 상태를 유지하면서 제어를 반환했다가(suspend) 제어를 다시 획득(resume)하며 흐름을 이어갈 수 있는 제어장치
-   제어 흐름에 관련된 용어(if, else)
-   Function은 suspend(yield) / resume이 빠진 것

  

## Generator란 무엇인가

-   Loop의 반복동작을 제어하는데 사용
-   iterator
-   흐름 제어하는 측면에서 구현

  

## Coroutine을 구현하는 방법

-   Generator yield는 다른 Coroutine으로 점프하지 않고 상위 루틴(dispatcher)으로 값을 넘김
-   Dispatcher에서 Generator가 반환한 토큰으로 다른 Generator를 실행하는 방식으로 구현

  

## Coroutine의 종류

-   Async Await 코루틴 (C#, JS)
-   continuations (연속체)
-   명령의 실행 순서를 완전히 제어할 수 있는 구문
-   지금 실행시킨 함수 호출이 끝나고 "발생시킨 함수" 혹은 "발생시키기 이전 함수"로 점프하는데 사용

  

### coroutine + scheduler = Fiber

(제어흐름을 위한 언어적요소 + 시스템적요소 = fiber)

##   Java Fiber

-   Java에선 Coroutine 지원 안함
-   그러나 Quasar를 통해 가능

##   Quasar

-   Java에서 Fiber를 사용하게 만들어 주는 라이브러리
-   JVM에서는 실행 상태 정보에 접근할 수 있도록 허용하지 않기 때문에 ..

  

## Fiber 어디에 써봤나?


실시간 게임 서버 엔진 Tardis

실시간 게임 서버는 statefull한 특징이 있다.  
짧은 응답시간, 멀티태스킹이 필요.

  
싱글스레드 fiber. fiber 스케쥴러 존재. async 필요없음

  
싱글스레드 프로세스를 멀티스레드 서버에 돌리는건 자원 낭비가 심하므로, 서버에 여러 노드를 만들어 병렬처리함

  

JS async await 문법처럼 사용하게 됨. 경험이 부족한 주니어개발자도 실수없이 로직을 짤 수 있게 되었다고 함.

  

요청을 처리하다 blocking 로직을 만났을 때, 그 유저의 fiber는 멈추지만 다른 유저들의 fiber는 동작중. 멈춘 fiber가 재실행되면 그 유저의 로직은 계속됨.

  

## Quasar의 단점

Fiber에서 thread blocking io 함수를 호출하면 안됨 (JDBC같은것)

  

## Project Loom

Quasar를 만든 사람이 주도하며 진행되고있음. (Quasar는 버전업 중단되었다고함)
