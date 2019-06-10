# 그런 REST API로 괜찮은가

[Naver DEVIEW 2017 : 이응준님 발표](https://tv.naver.com/v/2292653)를 들으며 듬성듬성 정리한 것입니다.

- REST란?
컴퓨터 시스템간의 상호 운용성을 제공하는 방법 중 하나

SOAP 과 REST가 경쟁하는 듯 했으나, SOAP는 복잡하고 불편해서 REST가 이김.

REST가 대세였긴 한데, REST 창안자 로이 필딩이 말하는 REST API를 제대로 구현하는 사례가 없었다.

REST API : REST 아키텍쳐 스타일을 따르는 API.
분산 하이퍼미디어 시스템(예: 웹)을 위한 아키텍쳐 스타일(인데, 스타일들의 집합임)

REST를 구성하는 스타일
- client-server
- stateless
- cache
- uniform interface
- layered system
- code-on-demand (optional) : 서버에서 코드를 클라이언트에 보내서 실행할 수 있어야 한다 (자바스크립트처럼)

uniform interface의 제약조건
- identification of resources : 리소스가 uri로 식별되어야 한다
- manipulation of resources through representations : representation 전송을 통해서 리소스를 조작해야 한다
- self-descriptive messages : 못지키고있다
- hypermedia as the engine of application state (HATEOAS) : 못지키고있다

self-descriptive message
```
GET / HTTP1.1
Host: www.example.org  // <- 목적지까지 들어가야 self-descriptive하다
```
```
HTTP/1.1 200 OK
Content-Type: application/json-patch+json      <- 내용이 어떤형식인지 나타내야 self-descriptive하다
[{"op":"Remove, "path":"/a/b/c"}]
// 게다가, op가 뭔지, path가 뭔지 이해 못하니까 json-patch+json 이라고 형식을 알면 내용을 알수있게 만들어야한다
```

HATEOAS
애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.
: HTML은 하이퍼링크를 통해서 다른 기능(화면)으로 넘어가므로 HATEOAS 만족한다.

---

SOAP가 어렵고 규칙도 많아서 REST가 성공했지만, 알고보면 REST도 어렵다.

REST는 HTTP와 웹의 진화, 독립적 진화에 큰 영향을 주었지만..
REST를 제대로 지켜 API를 만드는 것은 너무 어려운(혹은 비효율적인) 일이다.

> 시스템 전체를 통제할 수 있다고 생각하거나, 진화에 관심이 없다면, REST에 대해 따지느라 시간을 낭비하지 마라
-- 로이 필딩


오늘날, 다들 REST API가 아니지만 REST API라고 부른다.
로이 필딩은, 제발 제약 조건을 따르던지, 아니면 다른 단어를 쓰라고 말하고있지만..


**결론은,** (JSON을 갖고도) REST API를 만들기 위한 방법은 분명 여럿 존재하지만 단점들이 있고..
이미 REST아닌 REST API들이 보편화 되어있으니 너무 애쓰지 않아도 된다.

