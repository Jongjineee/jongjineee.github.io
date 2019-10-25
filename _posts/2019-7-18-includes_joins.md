---
layout: post
title: 레일즈 Joins VS Includes
image: https://drive.google.com/uc?id=1o28Np0nb4uQRGblbpKkw7dmlyiuHiaVd
date: 2019-7-18
categories: []
---
![ruby](https://drive.google.com/uc?id=1o28Np0nb4uQRGblbpKkw7dmlyiuHiaVd)

## Active Record
* * *

액티브레코드는 관계형데이터베이스(RDBMS)의 테이블을 객체로 연결(ORM : Object Relational Mapping)해서
네이티브 데이터베이스 SQL을 사용하지 않고도 데이터를 조작할 수 있도록 다양한 메소드를 제공해 줍니다.
액티브레코드에는 개발자가 쉽게 데이터를 조작할 수 있도록 도와주는 많은 메소드가 있습니다.  
이 액티브레코드의 메소드들 중 Includes와 Joins에 대해서 공부한 내용을 정리했습니다.
Includes와 Joins 메소드는 결과가 비슷하여 둘의 차이를 잘 모르고 사용하는 경우가 많습니다.

## Joins와 Includes의 차이
* * *

```ruby
users = User.where(status: 'active').joins(:profile)
users.each do |user|
  user_date << {
    name = user.name
    age = user.profile.age
  }
end
```

첫행에서 joins 메소드를 사용하여 users를 검색합니다.  
이후 do 루프에서 user의 name과 age를 받습니다.
위 코드는 얼핏보기에 이상한 점이 없으며 결과또한 별다른 에러 없이 잘 나옵니다.
하지만 이 코드를 실행해보면 처음 users를 검색하면서 1번의 쿼리가 실행되고, 이후 do 루프를 통해 N번의 쿼리가 실행됩니다.
이러한 로딩 방식을 lazy loading이라고 부르며 이로인해 발생하는 문제를 N+1 문제라고 부릅니다.

N+1 문제는 데이터의 규모에 따라 심각한 문제가 될 수 있습니다. users의 수가 100개라고 하면 101번의 쿼리가 실행되는데
이는 속도의 차이를 별로 못느낄 수 도 있겠지만 그 수가 10만명만 되어도 쿼리 속도에 문제가 있다는 것을 금방 알아차릴 수 있습니다.
한번 코드를 실행하면 10만 1번의 쿼리를 실행하게 되니까요.

Rails에서는 이러한 문제를 해결하기위해 Includes라는 메소드를 제공해줍니다.
Rails의 공식문서를 보면 Inlcludes에 대해서 아래와 같이 설명합니다.

> # With includes, Active Record ensures that all of the specified associations are loaded using the minimum possible number of queries

어째서 위와같이 쿼리를 줄일 수 있다는 것인지 위에서 사용한 Joins의 자리에 Includes를 사용해보겠습니다.

```ruby
  users = User.where(status: 'active').includes(:profile)
  users.each do |user|
    user_date << {
      name = user.name
      age = user.profile.age
    }
  end
```

위 코드를 실행해보면 두번의 쿼리가 발생합니다.  
users 데이터를 찾을 때 한번, users에 대한 profile을 찾는데 한번의 쿼리가 발생합니다.
이러한 로딩 방식을 eager loading이라고 부릅니다.

모든 상황에서 Incldues가 Joins보다 효율적이라면 Joins 메소드가 있을 이유가 없을 것입니다.  
Joins 메소드가 Includes 보다 효율적으로 사용되는 경우를 알아봅시다.

```ruby
  users = User.joins(:profile).where('profile.status = ?', 'active')
  users.each do |user|
    user_date << {
      name = user.name
    }
  end
```

Joins와 Includes를 설명하면서 보여드린 코드와는 조금 다릅니다.  
첫행에서 'active' 상태의 users를 검색하고 do 루프에서는 profile의 데이터를 사용하지 않습니다.
profile은 단지 filter의 역할을 하는 것입니다.