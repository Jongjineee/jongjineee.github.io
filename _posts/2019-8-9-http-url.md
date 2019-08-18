---
layout: post
title: HTTP 완벽가이드::URL
date: 2019-8-9
categories: []
---

이 포스팅은 책 'HTTP 완벽가이드'를 읽고 작성하였습니다.  
  
<br>

### 2. URL과 리소스

우리의 도시는 각기 다른 이름들에 대한 작명 표준에 동의하기 때문에, 도시에 있는 소중한 리소스를 모두 쉽게 공유할 수 있습니다.  
URL(Uniform Resource Locator)은 이와 같이 인터넷의 리소스를 가리키는 표준이름입니다. URL은 전자정보 일부를 가리키고 그것이 어디에 있고 어떻게 접근할 수 있는지 알려줍니다.

#### 2.1 인터넷의 리소스 탐색하기

앞서 말했듯 URL은 브라우저가 정보를 찾는데 필요한 리소스의 위치를 가리키며, URL을 이용해 사람과 애플리케이션이 인터넷상의 수십억 개의 리소스를 찾고 사용하며 공유할 수 있습니다.
또 URL을 통해 사용자는 HTTP 및 다른 프로토콜을 통해 접근할 수 있습니다. 
URL은 URI라고 불리는 더 일반화된 부류의 부분집합입니다. URI는 두 가지 주요 부분집합인, URL과 URN으로 구성된 종합적인 개념입니다. 
URL을 사용하면 리소스를 일관된 방식으로 지칭할 수 있습니다. 대부분의 URL은 동일하게 <span class="emphasis">‘스킴://서버위치/경로’</span> 구조로 이루어져 있습니다. 

***

#### 2.2 URL 문법

URL문법은 <span class="emphasis">스킴</span>에 따라서 달라집니다.  
다른 URL 스킴을 사용한다는 것이 전혀 다른 문법을 사용한다는 뜻은 아닙니다. 대부분의 URL은 일반 URL문법을 따르며, 서로 다른 URL 스킴도 형태와 문법 면에서 매우 유사합니다.
대부분의 URL 스킴의 문법은 일반적으로 9개 부분으로 나뉩니다.
<br>
<span class="emphasis"><스킴> : // <사용자 이름> : <비밀번호> @ <호스트> : <포트> / <경로>;<파라미터>?<질의>#<프래그먼트></span>
<br>

***

**`스킴`** : 리소스를 가져오려면 어떤 프로토콜을 사용하여 서버에 접근해야하는지 가리킨다.  

**`사용자이름`** : 몇몇 스킴은 리소스에 접근을 하기 위해 사용자 이름을 필요로 한다.  

**`비밀번호`** : 사용자의 비밀번호를 가리키며, 사용자 이름에 콜론(:)으로 이어서 기술한다.  

**`호스트`** : 리소스를 호스팅하는 서버의 호스트 명이나 IP 주소  

**`포트`** : 리소스를 호스팅하는 서버가 열어놓은 포트번호, 많은 스킴이 기본 포트를 가지고 있다(HTTP의 기본 포트는 80이다.)  

**`경로`** : 이전 컴포넌트와 빗금(/)으로 구분되어 있으며, 서버 내 리소스가 서버 어디에 있는지 가리킨다.  

**`파라미터`** : 특정 스킴들에서 입력 파라미터를 기술하는 용도로 사용한다. 파라미터는 이름/값을 쌍으로 가진다. 파라미터는 여러개를 가질 수 있다.  

**`질의`** : 스킴에서 애플리케이션에 파라미터를 전달하는데 쓰인다. Ex) https://www.joes-hardware.com/inventory-check.cgi?item=12731&color=blue&size=large  

**`프래그먼트`** : 리소스의 조각이나 일부분을 가리키는 이름이다.  

***

#### 2.3 단축 URL

웹 클라이언트는 몇몇 단축 URL을 인식하고 사용합니다. 

##### 2.3.1 상대 URL

URL은 상대 URL과 절대 URL 두 가지로 나뉩니다. 상대 URL을 통해 리소스에 접근하기 위해서는 <span class="emphasis">기저(base) URL</span>이 필요합니다. 
HTML 작성 시 <span class="emphasis">./hammers.html</span> 로 작성하는 URL을 상대 URL이라고 합니다. 

**`기저url`** : **http://www.joes-hardware.com/tools.html**

**`상대 url`** : **./hammers.html**
											
**`새로운 절대 url`** : **http://www.joes-hardware.com/hammers.html**

***

##### 2.3.2 URL 확장

어떤 브라우저들은 URL을 입력한 다음이나 입력하고 있는 동안에 자동으로 URL을 확장합니다. 이는 사용자가 URL을 빠르게 입력하게 도와줍니다.

* **호스트명 확장**  
호스트명 확장 기능을 지원하는 브라우저는 단순한 단어로 입력한 호스트 명을 전체 호스트 명으로 확장할 수 있습니다. 
예를 들어 주소 입력란에 ‘yahoo’를 입력하면, 브라우저는 호스트 명에 자동으로 ‘www.’ 와 ‘.com’을 붙여서 ‘www.yahoo.com’을 만듭니다.

* **히스토리 확장**  
브라우저가 사용하는 또다른 기술은 과거에 사용자가 방문했던 URL의 기록을 저장해 놓는 것입니다. URL을 입력하면, 그 입력된 URL의 앞 글자들을 포함하는 완결된 형태의 URL들을 선택하게 해줍니다.

***

#### 2.4 안전하지 않은 문자

안전한 전송이란, 정보가 유실될 위험 없이 URL을 전송할 수 있다는 것을 의미합니다. 문자가 제거되는 일을 피하고자 URL은 상대적으로 작고 안전한 알파벳 문자만 포함하도록 허락합니다.
하지만 URL에 이진 데이터나 알파벳 외의 문자도 포함하려고 하는 경우가 많아지자 이스케이프라는 기능을 추가하여, 안전하지 않은 문자를 안전한 문자로 <span class="emphasis">인코딩</span>할 수 있게 하였습니다. 

##### 2.4.1 URL 문자 집합

역사적으로 많은 컴퓨터 애플리케이션이 US-ASCII 문자 집합을 사용해왔습니다. US-ASCII는 문자를 서식화하고 하드웨어상에서 신호를 주고받기 위해, 7비트를 사용하여 영문 자판에 있는 키 대부분과 몇몇 출력되지 않는 제어 문자를 표현합니다.
<span class="emphasis">이스케이프 문자열</span>은 US-ASCII에서 사용이 금지된 문자들로, 특정 문자나 데이터를 인코딩할 수 있게 함으로써 이동성과 완성도를 높였다.

##### 2.4.2 인코딩 체계

인코딩은 안전하지 않은 문자를 퍼센티지 기호(%)로 시작해, ASCII 코드로 표현되는 두 개의 16진수 숫자로 이루어진 <span class="emphasis">‘이스케이프’</span> 문자로 바꿉니다.

##### 2.4.3 문자 제한

몇몇 문자는 URL 내에서 특별한 의미로 예약되어 있습니다.  
어떤 문자는 US-ASCII의 출력 가능한 문자 집합에 포함되어 있지 않은 경우도 있습니다. 

***

URL은 강력한 도구입니다. 하지만 모든 것들이 그렇듯 완벽한 것은 아닙니다. 사실 URL은 주소일뿐 실제 이름이 아닙니다. 주소란 URL이 특점 시점에 해당 리소스가 위치하는 곳을 알려준다는 뜻입니다.
이러한 스킴은 리소스가 해당 URL에서 옮겨지면 그 URL을 더이상 사용할 수 없습니다.
사람처럼 리소스의 이름을 포함하여 다른 몇 가지 정보만 있으면 리소스의 위치가 바뀌더라도 위치를 찾을 수 있습니다.
  
<span class="emphasis">URN</span>은 객체가 옮겨지더라도 항상 객체를 가리킬 수 있는 이름을 제공합니다.  
<span class="emphasis">PURL</span>은 리소스의 실제 URL 목록을 관리하고 추적하는 리소스 위치 중개 서버를 두고, 해당 리소스를 우회적으로 제공하기때문에 URL로 URN을 제공할 수 있습니다.

![first_process](https://drive.google.com/uc?id=1trYGbVTSaoZNvNjftlzmYA57bgJyZKyv)
* 죠의 컴퓨터 가게의 URL이 무엇인지 리소스 리졸버에게 묻습니다. 리졸버로부터 리소스의 현재 위치를 받습니다.

![second_process](https://drive.google.com/uc?id=1-6bvCro3toUN2fOP6cpCG3iO6j4V2TEO)
* 실제 URL로 리소스를 가져옵니다.

***

한동안 URN 방식이 활용되었었습니다.  
URL에서 URN으로 주소 체계를 바꾸는 것은 매우 큰 작업입니다. 표준화는 매우 중요한 작업인 만큼 느리게 진행되기때문에 URN으로 전환하기 위한 모든 것이 준비되려면 시간이 걸립니다.
웹이 성장함에 따라 사용자들은 학습을 통해 고질적인 문제를 다루는 법을 익혀왔습니다. URL은 나름 한계를 가지고 있지만, 웹 개발 커뮤니티에서 이를 가장 긴급한 사안이라고는 이야기하지 않습니다.
URL은 현재는 물론 가까운 미래에도 인터넷에 있는 리소스를 명명하는 방법이 될 것입니다. 이를 대체할 수 있는 작명 스킴이 나오기 전까지는 계속 되며 다만, URL은 그 한계를 가진 상태에서, 그것을 해결할 수 있는 새로운 표준(아마도 URN) 같은 것들이 나오고 적용될 것입니다.