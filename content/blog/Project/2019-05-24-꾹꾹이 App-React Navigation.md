React Native로 모바일 앱 개발

공식 문서의 안내에 따라 expo로 boilerplate를 만들게 되었다.
```bash
$ expo init
```
명령을 실행하면 navigation이 없는 빈 스크린 / navigation을 포함한 boilerplate를 선택할 수 있고, 프로젝트에서는 앱의 다양한 기능을 구분짓기 위해 navigation이 필요했다.

이 글에서는 React Navigation을 이용해 화면 전환 구현을 어떻게 하는지, 인증과 자동 로그인 구현을 어떻게 했는지 돌아본다.

---

**범위를 잘 정해서 정말 필요한 내용만 쓰자**

기본적으로 3개의 스크린과 이 스크린을 이동하기 위한 하단 탭 네비게이터를 제공한다.

프로젝트의  화면 구성
1. 로그인 > 가입
2. 적립
3. 검색 > 매장 정보 > 토스하기
4. 로그아웃

\> 로 표현된 화면 전환은 스와이프 동작을 통해 뒤로 돌아갈 수 있어야 한다.

React Navigation에 포함된 네비게이터의 종류가 몇가지 있다.
1. SwitchNavigator
2. StackNavigator
3. 

- 인증 flow 구현
공식 문서에 [예제]([https://reactnavigation.org/docs/en/auth-flow.html](https://reactnavigation.org/docs/en/auth-flow.html))가 있다
