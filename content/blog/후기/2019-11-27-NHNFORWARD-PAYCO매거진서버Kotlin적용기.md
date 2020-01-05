---
title: NHN FORWARD 2019 - PAYCO 매거진 서버 Kotlin 적용기
date: 2019-11-27 21:52:23
category: 후기
---

2019-11-27


# PAYCO 매거진 서버 Kotlin 적용기


#### 유재은, 전현구 (nhn payco 서버개발팀)

  

서버사이드 Java to Kotlin 적용기.

  
기존 스택을 모두 유지하고 Java → Kotlin

  

spring framework 5.0부터 kotlin 공식지원하면서 spring도 kotlin 사용이 늘고있음

###   Kotlin Favorite Functions

1. null safe하다  
2. extension functions  
3. 자바 100% 호환성  
4. 데이터 클래스  
5. Higher Order Function  
6. ..

  
NHN에선 작은 기능에 부분적용 및 선적용 해보는중임

###   Java / Kotlin

-   자바는 field는 있지만 property는 없음. getter/setter로 함

-   `data class Article`.. 데이터 클래스 선언

-   자바에서 boxing된 int는 null 가능성. primitive는 null 불허.

-   IDE 기능으로 바이트코드를 실시간으로 볼 수 있고, decompile해서 kotlin코드가 java로 어떻게 해석되나 볼 수 있음

  

### Migration

자동변환이 좋긴한데  
- Lombok을 사용한 코드까지 커버해주지 않음.  
- 의도치 않은 kotlin 코드를 만들어내기도 함

  
Lombok은 property 지원을 위함. kotlin은 지원하니까 lombok을 걷어내자.  
(java w/ lombok → java w/o lombok → kt auto convert → refactored kt)

  

  

### Migration - Dependency Injection

`lateinit` 키워드를 사용하면, 나중에 초기화가 될것임을 알려주고, nullable로 선언하지 않아도 된다.  
(null로 초기화 해주면서 선언하면 계속해서 nullable 타입으로 남게 되므로 빈틈이 생긴다)

  

### Migration - static field/method

companion object를 선언하고 안에다가 변수와 메서드를 선언하면 자바의 static 필드, static 메서드와 동일하게 동작.  
그러나 Java코드에서 참조할 수 없게 됨  
변수앞에 `const` 키워드, 메서드도 특정 어노테이션을 붙이면 Java에서 참조 가능해진다.

  

##   Refactoring

-   when 문법  
    if-else를 when으로 바꿔주는 IDE기능 활용할수 있다  
    val imgUrl = when { ... 이렇게 할당해서 when의 결과를 바로 변수에 할당하도록 작성 가능하다
-   model  
    controller에서 template에 데이터 넘겨줄 때 사용. spring의 model을 kotlin 확장으로 넣음

##   결과보고

- build time, API 응답속도 큰 차이 없음.  
- 소나큐브 분석결과 코드 line 수 6%감소, text량 10%감소  
- 의존성 주입 부분 코드 감소  
- null point exception 핸들링 부분 코드 제거 (if-else, try-catch)  
- null safe를 확신할 수 있으므로 핸들링이 필요 없어진다  
- if-else를 when으로 대체  
- statement(구문) 수 16% 감소  
- cyclomatic Complexity(순환복잡도) 12% 감소 (분기 코드마다 복잡도가 상승하는편. NPE 핸들링 제거로 인함)

  

개발경험이 좋아짐. 작성 효율이 좋아지고, 가독성도 좋아짐 = 빠른 개발 + 유지보수 용이  
(물론 kotlin 학습이 충분히 된 이후에)
