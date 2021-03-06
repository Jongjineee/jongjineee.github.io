---
layout: post
title: 도메인을 통해 웹사이트에 접속하는 과정
image: https://drive.google.com/uc?id=1CUAPOGF1_9fleXNgUso93fUJrabFl_dn
date: 2019-10-28
tag: [네트워크, 도메인, domain, 웹사이트, DNS]
categories: []
---

웹 서버에는 IP 주소가 있습니다. 
우리가 어떤 웹사이트에 접속한다는 것은 해당 웹사이트의 IP 주소로 접속하여
그 웹 서버가 제공하는 데이터를 보는 것입니다.
때문에 도메인을 이용하지 않고 IP 주소를 입력하는 것으로도 웹사이트에 접속이 가능합니다.
하지만 우리가 모든 웹사이트의 IP 주소를 외우거나 연필로 공책에 하나하나 적어놓고 주소록에서 전화번호를 
찾듯 웹사이트에 접속할 수 없기 때문에 <span class="emphasis-org">도메인</span>을 이용합니다.

***

#### 도메인이란?
도메인은 네트워크상에서 컴퓨터를 식별하는 호스트명입니다.
지정된 IP조소를 알기쉬운 문자로 변환한 인터넷 상의 주소라고 할 수 있습니다. 

***

#### 브라우저가 도메인에 해당하는 IP를 찾는 순서
1. Local Cache를 검색한다.
2. Hosts 파일을 검색한다.
3. 도메인 네임서버를 검색한다.

* Local Cache
우리가 한번이라도 어떤 사이트에 접속한다면 브라우저는 해당 도메인의 IP를 저장합니다.
도메인을 입력할 때마다 매번 IP를 검색한다면 비효율적이기때문에 브라우저에서 저장해놓고 
다음에 접속할 때 cache에서 바로 찾아서 접속하게 됩니다.
자동완성 기능이 작동한다면 cache에 저장이 되어있는 것이겠죠?

* Hosts 파일
Local Cache에 도메인에 해당하는 IP가 없다면 Hosts 파일에서 찾습니다.
각 운영체제는 Hosts 파일을 갖고 있는데 이는 도메인과 해당 IP를 매핑합니다.
때문에 관리자권한으로 해당 파일을 수정한다면 도메인과 다른 IP주소로 접속이 가능합니다.
물론 자신의 컴퓨터에서만!

* 도메인 네임서버를 검색
마지막 단계로 hosts 파일에도 도메인에 해당하는 IP 주소가 없을 때 도메인 네임서버(DNS)를 검색합니다.

![DNS](https://drive.google.com/uc?id=1CUAPOGF1_9fleXNgUso93fUJrabFl_dn)

위의 그림을 보면 쉽게 이해를 할 수 있습니다. DNS 서버는 IP 주소와 도메인의 매핑 정보를 관리합니다.
때문에 우리가 DNS 서버에 도메인에 해당하는 IP 주소를 묻는 요청에 응답해줍니다.
게다가 DNS에도 캐시가 있어 자주 요청 받는 정보는 캐시로 관리합니다.
