---
layout: post
title: 운영체제 (Operating System / OS) 구조 & 동작
image: https://drive.google.com/uc?id=14deQUE0ykyNyS2jnnmzTbvdZaldeZomb
date: 2019-8-25
tag: [운영체제, os, operating system, 리눅스, linux, 구조]
categories: []
---
![운영체제](https://drive.google.com/uc?id=14deQUE0ykyNyS2jnnmzTbvdZaldeZomb)

### 운영체제의 구조

운영체제가 동작하는 원리는 불편함을 개선하면서 변화해왔습니다. 
DOS 시절 운영체제에서는 자원의 효율성이 굉장히 떨어졌습니다. 
우선순위가 앞서 있는 작업을 수행하는 동안 CPU가 사용되지 않음에도 불구하고 
뒤에 있는 작업은 CPU를 사용하지 못하고 앞의 작업을 기다려야했습니다. 
이렇게 유연하지 않은 구조는 컴퓨터 전체의 효율성을 떨어뜨렸습니다. 
그래서 나온것이 멀티 프로그래밍(Multiprogramming)입니다.

***

#### 멀티 프로그래밍(Multiprogramming)

멀티 프로그래밍은 여러 작업들이 동시에 메모리에 올라갑니다.
그리고 앞의 작업이 CPU를 사용하지 않을 때 CPU는 대기상태에 들어갑니다. 
대기상태에 들어간 CPU는 다른 작업에서 사용할 수 있습니다. 
이러한 방식은 전의 방식보다 유연성을 제공하며 효율성을 높입니다. 반대로 I/O가 
대기 상태에 들어갈 때 I/O가 필요한 작업에게 그 자원을 할당해 줍니다. 

하지만 멀티프로그래밍에도 단점이 있습니다. 작업들마다 자원의 사용에 시간 차이가
생기는 것입니다. 예를들어 먼저 온 작업이 CPU나 I/O를 사용하고 있을 시 다른 작업은 
해당 자원을 사용하지 못하는 것입니다. 그럼 CPU나 I/O 중 한 자원은 대기를 하면서 
자원의 낭비가 일어나는 것입니다. 이러한 낭비는 효율성을 떨어뜨리게 됩니다.

***

#### 멀티 테스킹(Multitasking)

멀티 테스킹은 멀티 프로그래밍의 확장입니다. 
각각 작업에 시간을 부여하고 CPU 작업을 하다가 그 시간이 지나가면 다른 작업에게 
CPU 자원을 할당해줍니다. 주어진 시간을 두고 번갈아가면서 자원을 사용하는 것입니다. 
이렇게 하게되면 시간 지연으로 인한 낭비를 줄일 수 있습니다. 
주어진 시간은 굉장히 짧으며 빈번한 Switching이 발생합니다.

***

### 운영체제의 동작

운영체제는 이전 포스팅에서 말했듯이 Interrupt를 기다리고 Interrupt가 발생시 움직입니다.
이러한 방식을 Interrupt-driven 방식이라고 합니다. 현재의 운영체제는 Interrupt에 의해 구동됩니다.

***

#### 시스템 콜(System Call)

사용자 프로그램이 작동하다가 운영체제에게 어떠한 요청을 하는 것을 시스템 콜(System Call)이라고
합니다. 즉, 사용자 <span class="emphasis">프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출</span>하는 것입니다. 

Interrupt가 발생하면 운영체제는 해당 주소를 저장하고 그 주소에서 대기합니다. 
그리고 운영체제는 <span class="emphasis">Interrupt Vector Table</span>에서 해당 Interrupt에 대한 번호를 찾습니다.
그 번호에 대응하는 행위로 <span class="emphasis">Interrupt Vector Routine</span>을 실행시킵니다.
Interrupt Vector Routine이 끝나면 운영체제는 저장해 놓았던 주소로 되돌아가 다음 Interrupt를 
기다리게 되는 것입니다. 
시스템 콜은 이러한 Software Interrupt(**Trap**)의 일종이라고 할 수 있습니다.

* Trap에는 ``Exception``이라는 것도 있는데 이는 프로그램이 **오류**에 의해서 Interrupt를 발생시키는 것입니다.
운영체제는 한 작업의 Error로 인해 자원을 계속해서 점유하는 일 등 효율성을 저해하는 요인으로부터
**보호**할 수단을 필요로 합니다. 그러한 방법에는 크게 두가지가 있습니다. 

***

#### Dual-Mode Execution

이 방법의 조건으로 H/W의 지원이 필요합니다. 시스템을 보다 안전하게 하는 것이 목적입니다. 
이는 ``Mode-Bit``이라는 것을 필요로 하는데, Mode에는 **Kernel Mode**와 **User Mode** 두 가지가 
있습니다.

사용자가 Kernel 상 작업, 즉 컴퓨터의 Core에 해당하는 작업을 직접 명령하고 수행하다가 
잘못하여 시스템 전체에 악영향을 끼칠 수 있습니다. 이를 방지하고자하는 방법입니다. 

각 명령어에 Mode-Bit을 심어 해당 명령어의 Mode-Bit와 형재 시스템 상의 Mode-Bit가 
같을 시에만 해당 명령어를 수행하게끔 합니다.
사용자가 Kernel을 사용해 어떤 작업을 하려고 한다면 Mode-Bit를 Kernel-Mode로 변경 후 
해당 작업을 실행하고 작업이 끝나면 다시 User-Mode로 변경됩니다. 
이렇게 Mode-Bit를 변경하는 행위가 위에서 설명한 System Call에 해당합니다. 

***

#### Timer

두번째 방법으로 Timer가 있습니다. Timer는 무한 루프나 자원의 독점을 막는 역할을 해냅니다.
Timer는 특정 시간이 지나면 Interrupt를 발생시킵니다. 운영체제는 Timer가 끝난 작업을 
이 Interrupt를 통해 종료시키고 실행 되기 전 Scheduling 작업 전에 Timer를 작동시킵니다.
