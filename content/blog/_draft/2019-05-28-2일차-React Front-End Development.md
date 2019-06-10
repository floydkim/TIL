# 2일차) React Front-End Development
CHEQUER 장기영
https://docs.google.com/presentation/d/1gSztygbf9YRwtmsdLDpDoPO2wk-QAOBOs6e6d45y3n0/edit?fbclid=IwAR2hkqxy21VwHQ_-RCIlmaPYu9cEXcjprI8xhDMLdD2ZN95iOeHWw1lZGnc#slide=id.p
2. React Front-End Development
- React + Typescript 프로젝트 셋팅
- Ant Design 설치 및 환경 구성
- 데모 프로젝트 만들기
- 스타일링 / 테마다루기
- 효율적인 코드 작성 (구조분해할당, Async / Await, Type)
- 드래그 & 리사이징 (Resizing, ellipsis, Drag & drop, Sub component auto resizing)

CHEQUER co-founder, front-end dev.
jsdev.kr 공동 운영
open source maintainer
최근엔 리액트 컴포넌트 만드는 데 집중 중


ant design이 결론이더라. ant design에서 less를 쓰니까 오늘 해볼거임.

## CRA는 왜 써야 하나요?
CRA없이 React 프로젝트를 만들려면 해야 하는 일들
- index.html 형식에 맞도록 만들기
- webpack 설치 및 설정(loader, devServer...)
- babel
- eslint
- react설치
- typescript설치
-..

리액트같은건 버전업이 빨라서 매번 업데이트 해줘야 하는 불편함



예제 소스 코드들은 여기에
https://github.com/thomasJang/react-fed
-> fork & clone 해서 보면, 브랜치를 나눠서 여러 예제를 담고 있음!


$ npm init react-app my-app --typescript
위 명령어로도 CRA 가능



craco
https://github.com/sharegate/craco

설치
$ npm install @craco/craco


less, scss 모든 로더를 다 쓰는 형태로 프로젝트를 만들어야, 갖다쓰는 것들도 모두 쓸수있다.
모든 툴을 다 받아들일 수 있는 시스템을 만드는게 좋다.

import './App.css';
import './test.less'; <- craco에서 craco-less 플러그인 필요
import './test.scss'; <- node-sass 필요


ant design
$ npm i antd styled-components @types/styled-components


antd는 css와 js가 분리된 프로젝트임. css를 따로 올려줘야 한다


inline javascript is not enabled 뜰텐데, craco.config 안에 javascriptEnabled true 옵션 줘야한다.

(쉬는시간)

### 회원가입 폼을 빙자한 로그인 폼을 만들어보자!
app.tsx에 뭐가 많으면 안됨 원래는.


antd의 form은 react의 컨트롤드 컴포넌트를 자동으로 만들어주고, validator가 잘 되어있다!!



### markdown

npm 모듈을 고를때, 모듈의 디펜던시가 너무 많으면 프로젝트에 안좋다.
(리액트 버전이 올라가서 프로젝트에서도 버전올릴때, 디펜던시가 발목잡을때가 있다)

다운로드 추이도 보는게 좋다.

npm i markdown-to-jsx
npm i @types/markdown-to-jsx


.md파일의 경로를 읽어오는 문제가 있는데, 
craco에서 raw-loader를 넣어줘야 한다.


### Pop over

flexbox 이제는 호환성 기반이 마련됐으니 써도되지 않을까. (미지원 브라우저 비율이 굉장히 낮아졌다)
direction 정하고 나면 그 방향이 justify, 직각방향이 align

<Popover
            trigger="click" // 클릭하면 열리게 설정가능. 자동으로 사라지지 않아서 inspection 할 수 있음
            // 검사해보니 div id="root" 밖에 있는 녀석임. 이런건 styled component로 제어 불가.
            // 찾아본 결과, 이 popover는 tooltip을 가지고 만든거라, 공식문서에 tooltip의 api문서를 보라고 되어있음.
            overlayClassName="my-antd-popover" // 이 속성을 주면 클래스 부여 가능! 이상한 기본모양을 바꿀수있다.

            content={<UserMenu />}
          >


### Modal 을 Promise로 만들어보자

ui 모달을 만들면 window.alert 등과 달리, 앱의 실행을 block하지 않으니
promise로 만들고, async await을 이용해 block을 구현하자.



### Resize

리사이징 위한 bar를 만들 때, 겉보기엔 1px이라도 사용자가 컨트롤하기 쉽게 하기 위해 :after 같은 pseudo element를 만드는데, 너비를 총 9 ( 4 1 4 ) 짜리 투명 바를 겹쳐놓는다.!!! 꿀팁이네

drag & drop 하는 경우가 아니므로 onDrag?로 이벤트 처리하면 리사이즈 바의 허상이 커서를 따라다닌다.
onMouseDown으로 하면 잘못하면 옆에 다른것들이 셀렉트되어 highlight됨 처리필요


UI 변화를 마우스 움직임 등 다발적인 이벤트에 따라 변경해 줄 경우, throttle 걸어주는게 좋다.
특히 리사이즈되는 컨테이너 안에 많은 내용이 들어갔을때 쓰로틀이 없으면 부하가 많이 걸림.
(리액트 자체적으로 쓰로틀을 걸기는 하지만 부족?하다고..)



수업끝!

======
질의응답
enum 이용해서 프로젝트에 쓰이는 변수명들? 관리를 하면 좋다.




