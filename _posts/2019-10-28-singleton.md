---
layout: post
title: 디자인패턴_Singleton Pattern(싱글톤 패턴)
image: https://drive.google.com/uc?id=1gPylgzGIIEutY299CsVHF4lfl3PurlwX
date: 2019-10-28
tag: [디자인패턴, 싱글톤패턴, SingletonPattern, python]
categories: []
---

![python](https://drive.google.com/uc?id=1gPylgzGIIEutY299CsVHF4lfl3PurlwX)

#### 디자인패턴
디자인 패턴이란 자주 나타나는 시스템 구조를 조금 더 쉽고 빠르게 설계하기위해
재이용하기 좋은 형태로 구조화한 것을 말합니다.

***

#### Singleton Pattern(싱글톤 패턴)
특정 클래스의 인스턴스가 하나만 만들어지고, 
어디서든지 그 인스턴스에 접근할 수 있도록 하기 위한 패턴입니다.
객체 중에는 Manager나 사용자 설정같이 하나만 있어도 되거나 오직 하나만 있어야되는 것들이 있습니다.
이런 객체의 경우 인스턴스를 2개 이상 만들경우 에러나 자원의 낭비를 야기하기때문에  
싱글톤 패턴을 사용합니다.
<small><span class="emphasis-org">일반적으로 클래스의 인스턴스는 여러가지가 생성될 수 있습니다.</span></small> 

싱글톤 패턴은 특정 클래스에 대해 객체 인스턴스가 하나만 만들어질 수 있도록 해 주는 패턴입니다. 
즉, 싱글톤 패턴의 핵심은 어떠한 상황에서든 해당 객체의 인스턴스는 하나만 존재해야 한다는 것입니다. 

```python
class Singleton(type):                          # type을 상속받음
    __instances = {}                            # class의 instance를 저장할 속성
    def __call__(cls, *args, **kwargs):         # class로 instance를 만들 때 호출
        if cls not in cls._instances:           # class로 instance를 생성하지 않았느지 확인
                                                # 생성하지 않았으면 instance를 생성하여 속성에 저장
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
            
        return cls._instances[cls]               # class로 instance를 생성했으면 instance 반환

class Hello(metaclass=Singleton):
    pass

a = Hello()
b = Hello()      
print(a is b)                                    # True : 인스턴스 a와 b는 같음
```

싱글톤 패턴을 만드는 여러가지 방법 중 Metaclass를 이용하는 방법이 있습니다. 

***

<span class="emphasis-org">Metaclass?</span>  
메타클래스(Metaclass)는 클래스를 만드는 클래스입니다.
파이썬에서는 클래스도 객체이며 클래스를 만드는 또 다른 클래스가 메타클래스입니다.  
여기서는 간단히 설명하고 메타클래스에 대해서는 추후에 자세히 다루겠습니다. 
일반적으로는 자주 사용할 일이 없지만 Django 에서는 ORM을 다루는데 메타클래스가
꽤 중요한 역할을 해냅니다.

***

먼저 type을 상속받은 메타클래스 Singleton을 만들고, 클래스 Hello를 만들 때 metaclass에 Singleton을 
지정합니다. 이렇게 하면 메타클래스 Singleton 이 클래스 Hello의 동작을 제어할 수 있습니다. 

보통 `__call__` 메서드는 인스턴스를 ()로 호출할 때 호출됩니다. 
하지만 메타클래스에서 `__call__` 메서드를 구현하면
메타클래스를 사용하는 클래스로 인스턴스를 만들 때 `__call__` 메서드가 호출됩니다. 

여기서는 Hello()로 인스턴스를 만들 때 Singleton의 `__call__` 메서드가 호출됩니다. 
따라서 `__call__` 안에서 중복을 확인하고, 중복이 없으면 인스턴스를 생성,
속성에 저장한 뒤 반환합니다. 만약 인스턴스가 생성되어 있다면 인스턴스를 더이상 생성하지 않고
바로 반환합니다.
