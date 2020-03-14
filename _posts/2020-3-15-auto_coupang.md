---
layout: post
title: "[Python] 쿠팡 파트너스 자동 포스팅 개발기 - 1"
image: https://drive.google.com/uc?id=1gPylgzGIIEutY299CsVHF4lfl3PurlwX
date: 2020-3-15
tag: [블로그, 쿠팡, 쿠팡 파트너스, API, python, 크롤링, 토이 프로젝트, selenium]
categories: []
---

![python](https://drive.google.com/uc?id=1gPylgzGIIEutY299CsVHF4lfl3PurlwX)

### 쿠팡 파트너스 상품 가져오기 <a href="https://github.com/Jongjineee/auto_coupang_partners" target="_blank"><i class="fab fa-github"></i></a>

***

‘쿠팡 파트너스’는 쿠팡에서 운영하는 온라인 제휴마케팅 서비스입니다. 개인의 홈페이지, 블로그, SNS 등을 사용하여 쿠팡 상품을 노출하여 구매가 발생하면 광고비를 지급합니다. 즉, 쿠팡의 제품을 쿠팡 파트너스를 통해 링크를 발급받아 공유하고, 그 링크를 통해 구매가 발생하면 광고비를 얻을 수 있는 것입니다. 쿠팡 파트너스에서는 사용자가 적용할 수 있는 쿠팡의 상품 정보를 포함한 파트너스의 기능과 실적 정보 등을 API로 제공합니다. 이 쿠팡 파트너스와 네이버 블로그 API를 이용하여 쿠팡 상품을 네이버 블로그에 자동 포스팅하는 
Python 코드를 만들어 보았습니다.
<br>
<br>
[∙ 쿠팡 API](https://partners.coupang.com/#help/open-api)
<br>
[∙ 네이버 블로그 API](https://developers.naver.com/products/blog/)
<br>
<br>
코드는 <span class="emphasis">main.py</span>를 통해서 실행할 수 있으며 <span class="emphasis">auto_coupang.py</span>를 통해 쿠팡 파트너스의 상품 데이터를 가져오고 
<span class="emphasis">auto_naver.py</span>를 통해 상품을 블로그에 포스팅합니다.

***

#### HMAC 인증

```python
def generate_hmac(method, url, secret_key, access_key):
    path, *query = url.split("?")
    os.environ["TZ"] = "GMT+0"
    datetime = time.strftime('%y%m%d')+'T'+time.strftime('%H%M%S')+'Z'
    message = datetime + method + path + (query[0] if query else "")
    signature = hmac.new(bytes(secret_key, "utf-8"),
                        message.encode("utf-8"),
                        hashlib.sha256).hexdigest()

    return f"CEA algorithm=HmacSHA256, access-key={access_key}, signed-date={datetime}, signature={signature}"
```

쿠팡 API를 통해서 상품 정보를 요청할 때 쿠팡은 <span class="emphasis">HMAC 인증 방식</span>을 통해 클라이언트를 인증합니다. HMAC인증은 REST API 요청을 받을 때, 요청의 중간 과정에서 위변조가 있었는지 알아내는 방법입니다. 해싱 기법을 적용하여 메시지의 위변조를 방지하는 기법을 <span class="emphasis">HMAC(Hash-based Message Authentication)</span>이라고 합니다.<br>
과정을 차례대로 살펴보면

1. 사전에 서버와 클라이언트는 별도의 채널에서 해시에 사용할 **Key**를 공유합니다.
2. 클라이언트는 **Key**와 **Message**를 HMAC 알고리즘으로 
**Hash** 값으로 만듭니다.
3. 생성된 **Hash** 값과 **Message**를 **HTTP 요청**으로 서버에 보냅니다.
4. 서버는 클라이언트에게 받은 요청의 **Message**와 본인이 가진 **Key**를 조합하여 HMAC 알고리즘으로 클라이언트와 동일하게 **Hash** 값을 만듭니다.
5. 클라이언트에서 받은 **Hash** 값과 서버에서 생성한 **Hash** 값을 비교합니다.

클라이언트에서 정상적으로 생성된 Hash 값을 재사용하는 것을 막기위해 Hash를 생성할 때 **TimeStamp**를 포함하는 것이 좋습니다. 
`generate_hmac`으로 Hash 값을 생성했다면 `get_product`로 상품 정보를 가져옵니다. 
**Requests 모듈**을 이용해서 HTTP 요청을 보냅니다. 요청의 헤더에는 앞서 인증을 받기위해 생성한 Hash 값을 넣어줍니다.

***

#### 퍼센트 인코딩

요청을 보낼 때 URL은 쿠팡 파트너스의 검색 기능을 이용합니다. 검색에는 <span class="emphasis">keyword</span>와 <span class="emphasis">limit</span> 파라미터가 사용됩니다. Limit는 integer 타입의 데이터를 받기 때문에 쉽게 넣을 수 있지만 keyowrd에는 <span class="emphasis">한글</span>을 입력하게 됩니다. URL에 사용할 수 있는 문자는 <span class="emphasis">ASCII 코드</span>로 표현할 수 있는 영문, 숫자, 몇몇 기호 뿐입니다. 때문에 한글은 사용할 수 없습니다.
예를들어 쿠팡에 ‘가구’를 검색하여 나온 URL을 `urllib.request.urlopen()`으로 요청하면 ‘가구’라는 한글 때문에 에러가 발생합니다.

```
>>> urllib.request.urlopen('https://www.coupang.com/np/search?component=&q=%EA%B0%80%EA%B5%AC&channel=user')
UnicodeEncodeError: 'ascii' codec can't encode characters in position 28029: ordinal not in range(128)
```

이를 피하기 위해 URL에서 ‘가구’라는 검색어를 퍼센트 인코딩(percent encoding)으로 인코딩해주어야 합니다. 우리가 사용하는 웹 브라우저에서는 퍼센트 인코딩을 자동으로 수행해주지만 파이썬에서는 `urllib.parse.quote()`로 한글을 ASCII 코드로 변환해 줍니다.

```
{
  "rCode": "0",
  "rMessage": "",
  "data": {
	"landingUrl": "https://link.coupang.com/re/AFFSRP?lptag=AF1234567&pageKey=fashion&traceid=V0-153-5243b8cd44933da5",
	"productData": [
  	{
    	"keyword": "Water",
    	"rank": 12,
    	"isRocket": false,
    	"productId": 27664441,
        "productImage": "http://static.coupangcdn.com/image/product/image/vendoritem/2018/03/12/3205353792/48d92887-82e7-46f5-8704-89df99a23bde.jpg",
    	"productName": "탐사 소프트 100% 천연펄프 3겹 롤화장지 30m, 30롤, 1팩",
	    "productPrice": 15600,
    	"productUrl": "https://link.coupang.com/re/AFFSDP?lptag=AF1234567&pageKey=319834306&itemId=1023216541&vendorItemId=70064597513&traceid=V0-163-5fddb21eaffbb2ef"
  	}
	]
  }
}

```

요청을 통해 위와 같은 형식의 데이터를 받게 되고 <span class="emphasis">‘productData’</span>에서 사용하고자 하는 데이터를 사용합니다.

***

#### Selenium
위에서 가져온 상품의 데이터만으로도 블로그에 상품을 포스팅 하기에는 충분합니다. 하지만 더 많은 정보를 가져오고 싶어서 상품 상세페이지를 <span class="emphasis">크롤링</span>하도록 했습니다. 크롤링을 위해 <span class="emphasis">Selenium</span>을 사용했습니다. Selenium은 <span class="emphasis">webdriver</span>를 통해 브라우저를 제어할 수 있습니다. 자신이 쓰고자 하는 브라우저의 driver를 설치합니다. 저는 Chrome driver를 설치했습니다.
<br>
<br>
[∙ Chrome driver](https://sites.google.com/a/chromium.org/chromedriver/downloads)
<br>
<br>
코드에서 <span class="emphasis">ChromeOptions</span> 객체를 생성하여 `add_argument()`를 통해 여러가지 옵션을 설정할 수 있습니다. 이중 <span class="emphasis">‘headless’</span>는 브라우저를 렌더링하지 않고 메모리 상에서만 작업이 이루어지도록 합니다. 
크롤링을 진행할 브라우저인 User-agent 정보를 입력하고 이전에 설치한 chromedriver의 경로를 <span class="emphasis">webdriverpath</span>에 넣어줍니다. 저는 코드와 같은 위치에 넣어주었습니다.
이후 설정한 옵션과 함께 chromedriver를 실행해줍니다. 이때 `time.sleep()`을 이용해 페이지 로딩이 완전히 이루어질 때까지 잠깐 멈춰 기다려줍니다.
<br>
<br>
`Driver.execute_script()`를 통해 실행하는 script 코드들은 <span class="emphasis">크롤러 탐지를 우회</span>하는 코드입니다. 플러그인 속성을 덮어써 임의의 배열을 반환하도록 하고, 언어 설정이 되어 있지 않은 Headless모드에 언어를 설정해줍니다. 또 그래픽카드가 돌아가는 척하도록 해줍니다. 사람인 척하는 것입니다.<br>
이렇게 봇이 아닌 척을 열심히 하고 페이지의 요소들을 가져옵니다. `Find_element_by_class_name`을 이용하면 쉽게 요소들을 가져올 수 있습니다. 이미지의 경우 `os.makedirs()`를 이용해 폴더를 생성하고 `urllib.request.urlretrieve()`를 이용해 이미지를 바로 파일로 저장할 수 있습니다.

***

Return 값에서 `img`의 값이 조금 특이한데 이는 이후 <span class="emphasis">네이버 블로그 API</span>를 다루는 편에서 자세히 알려드리겠습니다.
이렇게 상품을 가져오는 <span class="emphasis">auto_coupang.py</span> 코드를 만들었고 다음편에 <span class="emphasis">auto_naver.py</span>를 리뷰하겠습니다.
