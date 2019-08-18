---
layout: post
title: HTTP 완벽가이드::HTTP란?
date: 2019-7-18
categories: []
---

이 포스팅은 책 'HTTP 완벽가이드'를 읽고 작성하였습니다.  
  
<br>

#### HTTP란
&nbsp;&nbsp;&nbsp;&nbsp; HTTP는 신뢰성 있는 데이터 전송 프로토콜을 사용하기 때문에, 데이터가 전송 중 손상되거나 꼬이지 않음을 보장합니다. 
이는 개발자에게도 이롭습니다. HTTP 통신이 전송 중 파괴되거나, 중복되거나, 왜곡되는 것을 걱정하지 않아도 되기 때문에 우리는 데이터가 통신 중 잘못되는 상황을 걱정하지 않고 기능 개발에 집중할 수 있습니다.

<br>

#### 웹 클라이언트와 서버

&nbsp;&nbsp;&nbsp;&nbsp; 웹 서버는 인터넷의 데이터를 저장하고, HTTP 클라이언트가 요청한 데이터를 제공합니다.
클라이언트는 서버에게 HTTP 요청을 보내고 서버는 요청된 데이터를 HTTP 응답으로 돌려줍니다. 
HTTP 클라이언트와 HTTP 서버는 월드 와이드 웹의 기본 요소입니다.

![client-server](https://drive.google.com/uc?id=1h2hEPUZ6wLGnBfxxgGWKLlvmBycgokqa)

가장 흔한 클라이언트는 크롬이나 인터넷 익스플로러와 같은 웹 브라우저입니다.  
웹 브라우저는 서버에게 HTTP 객체를 요청하고 사용자의 화면에 보여줍니다.

<br>

#### 리소스

&nbsp;&nbsp;&nbsp;&nbsp; 웹 서버는 웹 리소스를 관리하고 제공합니다. 가장 단순한 웹 리소스는 웹 서버 파일 시스템의 정적 파일입니다. 
리소스에는 정적 리소스 뿐만 아니라 동적 리소스도 있습니다. 동적 리소스는 사용자가 누구인지, 어떤 정보를 요청했는지, 몇 시 인지에 따라 다른 콘텐츠를 생성하는 것을 말합니다. 요약하자면 어떤 종류의 콘텐츠 소스도 리소스가 될 수 있다는 것입니다.

<br>

* **미디어 타입** <br>
&nbsp;&nbsp;&nbsp;&nbsp; 인터넷은 수천 가지의 데이터 타입을 다루기 때문에, HTTP는 웹에서 전송되는 객체 각각에 신중하게 MIME 타입이라는 데이터 포맷 라벨을 붙입니다. 이는 원래 각기 다른 전자메일 시스템 사이에서 메시지가 오갈 때 겪는 문제점을 해결하기 위해 설계 되었습니다. HTTP에서도 멀티미디어 콘텐츠를 기술하고 라벨을 붙이기 위해 채택되었습니다.
웹서버는 모든 HTTP 객체 데이터에 MIME 타입을 붙입니다. 웹 브라우저는 서버로부터 객체를 돌려받을 때, 다룰 수 있는 객체인지 MIME 타입을 통해 확인합니다.
예를들어 HTML로 작성된 텍스트 문서는 text/html 라벨이 붙고, JPEG 이미지는 image/jpeg 가 붙습니다.

<br>

#### URI

&nbsp;&nbsp;&nbsp;&nbsp; 웹 서버 리소스는 각자 이름을 갖고 있기 때문에, 클라이언트는 관심 있는 리소스를 지목할 수 있습니다. 서버 리소스 이름은 URI로 불립니다. 이는 정보 리소스를 고유하게 식별하고 위치를 지정할 수 있습니다.

![http-uri](https://drive.google.com/uc?id=1D5IdjQoIy5wlrg9VzLFAqnZ8kkKd9Ggx)

<br>

* **URL** <br>
&nbsp;&nbsp;&nbsp;&nbsp; URL은 리소스 식별자의 가장 흔한 형태입니다. URL은 특정 서버의 한 리소스에 대한 구체적인 위치를 서술합니다.
URL의 첫 번째 부분은 스킴이라고 불리는데, 리소스에 접근하기 위해 사용되는 프로토콜을 서술합니다. 보통 HTTP 프로토콜입니다.
두 번째 부분은 서버의 인터넷 주소를 제공합니다.
마지막은 웹 서버의 리소스를 가리킵니다.
<br>
<br>
오늘날 대부분의 URI는 URL입니다!

<br>

#### 트랜잭션

&nbsp;&nbsp;&nbsp;&nbsp; HTTP 트랜잭션은 요청 명령과 응답 결과로 구성되어 있습니다.

<br>

#### 메서드

&nbsp;&nbsp;&nbsp;&nbsp; HTTP 요청 메시지는 한 개의 메서드를 갖습니다. 메서드는 서버에게 어떤 동작이 취해져야 하는지 말해줍니다.

`GET` | 서버에서 클라이언트로 지정한 리소스를 보내라
`PUT` | 클라이언트에서 서버로 보낸 데이터를 지정한 이름의 리소스로 저장하라
`DELETE` | 지정한 리소스를 서버에서 삭제하라
`POST` | 클라이언트 데이터를 서버 게이트웨이 애플리케이션으로 보내라
`HEAD` | 저장한 리소스에 대한 응답에서, HTTP 헤더 부분만 보내라

<br>

#### 상태 코드

&nbsp;&nbsp;&nbsp;&nbsp; HTTP 응답 메시지는 상태 코드와 함께 반환됩니다. 상태 코드는 클라이언트에게 요청이 성공했는지 아니면 추가 조치가 필요한지 알려주는 세 자리 숫자입니다.

<br>

#### 메시지

&nbsp;&nbsp;&nbsp;&nbsp; HTTP 메시지는 단순한 줄 단위의 문자열입니다. 이진 형식이 아닌 일반 텍스트이기 때문에 사람이 읽고 쓰기 쉽습니다.
클라이언트에서 웹 서버로 보낸 HTTP 메시지를 요청 메시지, 서버에서 클라이언트로 가는 메시지는 응답 메시지라고 부릅니다. 

<br>

#### TCP/IP

&nbsp;&nbsp;&nbsp;&nbsp; HTTP는 애플리케이션 계층 프로토콜입니다. HTTP는 네트워크 통신의 핵심적인 세부사항에 대해서 신경쓰지 않고, 대신 TCP/IP에게 맡깁니다.
TCP는 다음을 제공합니다.
1. 오류없는 데이터 전송
2. 순서에 맞는 전달(데이터는 언제나 보낸 순서대로 도착한다.)
3. 조각나지 않는 데이터 스트림 (언제든 어떤 크기로든 보낼 수 있다.)

인터넷은 이 TCP/IP에 기초하고 있습니다. 
TCP/IP는 각 네트워크와 하드웨어의 특성을 숨기고, 어떤 종류의 컴퓨터나 네트워크든 서로 신뢰성 있는 의사소통을 하게 해줍니다.

더 자세한 내용은 **[게시글]**을 참고하세요 :)

<br>

#### TCP/IP 커넥션

1. 웹브라우저는 서버의 URL 에서 호스트 명을 추출한다.
2. 웹브라우저는 서버의 호스트 명을 IP로 변환한다.
3. 웹브라우저는 URL에서 포트번호를 추출한다.
4. 웹브라우저는 웹 서버와 TCP 커넥션을 맺는다.
5. 웹브라우저는 서버에 HTTP 요청을 보낸다.
6. 서버는 웹브라우저에 HTTP 응답을 돌려준다.
7. 커넥션이 닫히면, 웹브라우저는 문서를 보여준다.

<br>

#### 웹의 구성요소

**`프락시`** 클라이언트와 서버 사이에 위치한 HTTP 중개자  
프락시는 클라이언트와 서버 사이에 위치하여, 클라이언트의 모든 HTTP 요청을 받아 서버에 전달합니다. 
응답과 요청의 필터링 역할을 하며 주로 보안을 위해 사용됩니다. 즉, 모든 웹 트래픽 흐름 속에서 신뢰할 만한 중개자 역할을 합니다.

**`캐시`** 많이 찾는 웹페이지를 클라이언트 가까이에 보관하는 HTTP 창고  
자신을 거처가는 문서들 중 자주 찾는 것의 사본을 저장해둡니다. 클라이언트는 멀리 떨어진 웹 서버보다 근처의 캐시에서 훨씬 더 빨리 문서를 다운 받을 수 있습니다.

**`게이트웨이`** 다른 애플리케이션과 연결된 특별한 웹서버  
게이트웨이는 주로 HTTP 트래픽을 다른 프로토콜로 변환하기 위해 사용됩니다.

**`터널`** 단순히 HTTP 통신을 전달하기만 하는 특별한 프락시  
터널은 두 커넥션 사이에서 데이터를 열어보지 않고 그대로 전달해줍니다. 
이를 활용하는 대표적인 예로 암호화된 SSL 트래픽을 HTTP 커넥션으로 전송함으로써 웹 트래픽만 허용하는 사내 방화벽을 통과시킵니다.

**`에이전트`** 자동화된 HTTP 요청을 만드는 준지능적 웹클라이언트 (봇)

[게시글]: https://jongjineee.github.io/datascience/2018/12/31/tcp_ip.html "TCP/IP"