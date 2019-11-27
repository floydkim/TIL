---
title: NHN FORWARD 2019 - 벅스 5.0 (feat. Kotlin, Jetpack)
date: 2019-11-27 21:43:23
category: 후기
---

2019-11-27

# 벅스 5.0 (feat. Kotlin, Jetpack)

#### 유재설 이준호 장혜진 (nhn bugs android팀)

##   4++

* 앞으로 버전들에 대한 준비가 필요. 레거시 탈피하기  
* 개편, 코드쪼개기

유지보수성 좋아지고, class, method, field 증가. (목표를 달성했다고 했는데, 늘어난게 왜 좋은건진 모르겠습니다)

  

## Java to Kotlin

코드 라인수 감소함.

####   Migration

- Android Studio의 convert to Kotlin 기능  
- plug-in (JSON to Kotlin Class, Parcelable)  
- nullable

  
Nullable  
처리를 똑바로 하지 않으면 배포가 불가하다

##   AAC(Android Architecture Components)

- MVC to MVVM (AAC에서 제공하는 MVVM으로 갔다고 함)  
- 다른 팀과의 협업 고려 + View와 비즈니스 로직 분리 (UI담당하는 다른팀이 있다고 함)  
- Learning Curve 고려해서 선택

  

### Data Binding

도입 이유  
- 뷰 관련 반복 코드 제거  
- Observable을 이용해 데이터 변경을 UI에 반영

  

  

### AAC ViewModel

Jetpack

폰트변경, 다크모드 변경에 이점

  
re-create 후 데이터를 다시 불러오지 않아도 된다.

  

###   Lifecycle

  
processLifecycleOwner로 앱의 background.foreground 상태에 따른 로직 실행 가능

  

## Media app architecture

- 벅스 뮤직의 복잡한 비즈니스 로직  
- 다양항 UI 클라이언트들 (차량, chromecast, bigsby 등..)

MAA 이용해 파편화된 코드를 합칠 수 있었다

  
공식 아키텍쳐이므로 벅스를 사용할 벤더들은 인터페이스에 맞게 구현하면 됨. 따로 문서화해주지 않아도 돼서 좋다.
