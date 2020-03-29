---
layout: post
title: "[Python] 쿠팡 파트너스 자동 포스팅 개발기 - 2"
image: https://drive.google.com/uc?id=1gPylgzGIIEutY299CsVHF4lfl3PurlwX
date: 2020-3-29
tag: [블로그, 쿠팡, 쿠팡 파트너스, API, python, 크롤링, 토이 프로젝트, selenium]
categories: []
---

![python](https://drive.google.com/uc?id=1gPylgzGIIEutY299CsVHF4lfl3PurlwX)

### 네이버 블로그에 자동 포스팅하기 <a href="https://github.com/Jongjineee/auto_coupang_partners" target="_blank"><i class="fab fa-github"></i></a>

***

이전 포스팅 [Python] 쿠팡 파트너스 자동 포스팅 개발기 - 1 에서 쿠팡 파트너스의 API를 이용해 상품의 데이터를 가져오는 방법에 대해서 알아 보았습니다.<br>
이번 포스팅에서는 네이버의 API를 이용하여 이전에 받아온 쿠팡 파트너스의 상품을 네이버 블로그에 포스팅 해보겠습니다.

***

#### 네이버 로그인

```python
def get_logintoken(id, pw, client_id, client_secret, callback_url):
    options = webdriver.ChromeOptions()
    options.add_argument('headless')
    options.add_argument("user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.87 Safari/537.36")
    webdriver_path = os.getcwd() + r"/chromedriver"

    driver = webdriver.Chrome(webdriver_path, options=options)
    driver.implicitly_wait(5)

    driver.get('https://nid.naver.com/nidlogin.login')
    driver.execute_script("document.getElementsByName('id')[0].value=\'"+ id + "\'")
    driver.execute_script("document.getElementsByName('pw')[0].value=\'"+ pw + "\'")
    driver.find_element_by_xpath('//*[@id="label_login_chk"]').click()
    driver.find_element_by_xpath('//*[@id="frmNIDLogin"]/fieldset/input').click()
    driver.implicitly_wait(10)
```
네이버 블로그에 포스팅 하기 위해서는 로그인이 필요합니다. `get_logintoken()`함수는 id, pw 등 로그인 인증을 위한 인자를 받아 로그인 하는 역할을 합니다. 자동으로 로그인을 하도록 이번에도 webdriver를 이용했습니다. 네이버 로그인 페이지에서 ID와 PW를 입력하고 로그인 버튼을 누르도록 합니다. `implicitly_wait()`를 이용하여 페이지가 열리는 시간동안 잠시 기다렸다가 코드가 실행되도록 해줍니다.

***
#### 네이버 로그인 API 토큰
네이버 블로그 API를 이용하기 위해서는 [네이버 아이디 로그인 API](https://developers.naver.com/docs/login/api/)를 이용해 접근 토큰(access token)을 받아야 합니다.

```python
url = 'https://nid.naver.com/oauth2.0/token?'
data = 'grant_type=authorization_code' + '&client_id=' + client_id + '&client_secret=' + client_secret + '&redirect_uri=' + callback_url + '&code=' + code + '&state=' + state
request = urllib.request.Request(url, data=data.encode("utf-8"))
request.add_header('X-Naver-Client-Id', client_id)
request.add_header('X-Naver-Client-Secret', callback_url)

response = urllib.request.urlopen(request)
rescode = response.getcode()

if rescode == 200:
  response_body = response.read()
  js = json.loads(response_body.decode('utf 8'))
  token = js['access_token']
  return token
else:
  print("Error Code:", rescode)
  return ""
```

토큰을 받기위한 Request를 URL은 위 링크에 자세히 나와있습니다. 간단히 설명하자면 client_id와 client_secret, callback_url 등의 data를 포함하여 Request를 보내고 받은 response를 확인합니다. 
```
Response Example
{'access_token': '', 'refresh_token': '', 'token_type': 'bearer', 'expires_in': ''}
```
위와 같이 response를 받게 되면 access_token을 사용할 수 있게 됩니다.

***

#### 네이버 블로그 API
위에서 네이버 로그인 API로 access_token을 받았다면 네이버 블로그 API를 사용할 수 있게 됩니다.

```python
def write_product(access_token, title, category_number, contents, img):
  blog_header = "Bearer " + access_token # Bearer 다음에 공백 추가
  blog_write_api = "https://openapi.naver.com/blog/writePost.json"
  blog_data = {'title': title, 'categoryNo': category_number, 'contents': contents}
  blog_headers = {'Authorization': blog_header }

  response = requests.post(blog_write_api, headers=blog_headers, files=img, data=blog_data)
  rescode = response.status_code
  return rescode
```
* Bearer Authentication란?<br>
API에 접속하기 위해서는 access token을 API 서버에 제출해서 인증을 해야 합니다. 이 때 사용하는 인증 방법이 Bearer Authentication 입니다. 이 방법은 OAuth를 위해서 고안된 방법이고, [RFC 6750](https://tools.ietf.org/html/rfc6750)에 표준명세서가 있습니다.

access_token을 전송할 때 토큰을 인증하기 위한 방법을 포함한 요청 변수들을 넣어줍니다. 사용할 수 있는 기능들은 [네이버 블로그 API](https://developers.naver.com/docs/blog/post/)에서 확인하실 수 있습니다. 이전 포스팅에서 image에 대해서 특이한 점이 있다고 말했는데 
```
img = [('image', (fname, open(fnamefull, 'rb'), 'image/png', {'Expires': '0'}))]
```
위와 같이 img 리스트에 여러 이미지를 multipart request로 전송해야 된다고 합니다. 덕분에 contents를 작성할 때 img 태그에 src="#숫자"와 같은 형태로 쉽게 불러와 사용할 수 있습니다.
이제 write_product로 Request API를 보내면 네이버 블로그에 자동으로 포스팅이 됩니다.

***

#### 실적
코드가 까다롭거나 어렵지 않아서 쉽게 이해할 수 있을 것입니다. 코드는 Github에 올려 놓았으니 누구나 사용하실 수 있습니다. 포스팅하는 콘텐츠를 좀 더 성의있게 꾸며야 되는데 미루다가 결국 못꾸미고 그대로 사용해버렸습니다. 그래서 그런지 실적이 너무 저조하네요..😭 그래도 코드를 실행한 3월 9일부터 29일까지 약 20일간 자동화 코드를 실행하고 발생한 실적을 공개합니다.🙂
![쿠팡실적](https://drive.google.com/uc?id=15Sg3byr_EAEzGnNo4rv1SLKZHFezfdGO){: .center}

