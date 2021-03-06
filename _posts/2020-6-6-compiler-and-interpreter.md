---
layout: post
title: "컴파일러(Compiler)와 인터프리터(Interpreter)"
image: 
date: 2020-6-6
tag: [compiler, interpreter, cs, computer science, programming, 컴파일러, 인터프리터, 프로그래밍 언어]
categories: []
---
![intro](https://drive.google.com/uc?id=12WOTdHsI0mUMPmGjeLpJmw9lzVSDtBVX){: .center}{: width="100%"}
<br>
개발을 시작할 때 가장 먼저 배우는 것은 프로그래밍 언어입니다.
우리는 컴퓨터가 이해할 수 있는 언어인 프로그래밍 언어를 컴퓨터에 입력해 만들고자 하는 어떤 제품이나 서비스를 만들게 됩니다.
하지만 우리는 컴퓨터가 이진수(0과 1로 이루어진 수)만 이해할 수 있다고 알고 있습니다. 
이는 컴퓨터의 CPU가 인식해서 기능을 이해하고 실행할 수 있는 기계어 명령어가 이진수로 이루어져있다는 것을 의미합니다.
이렇게 CPU가 이해할 수 있는 기계어 명령어를 모아놓은 것을 기계어 <span class="emphasis">명령어 셋(Instruction Set)</span>이라고 하며, 여러 기계어 명령어 셋으로 구성한 구조를 <span class="emphasis">ISA(Instruction Set Architecture)</span>라고 합니다. 이런 명령어 셋은 각 CPU 회사 (인텔, AMD 등)에서 만들어내고 때문에 각 회사의 CPU 별로 명령어 셋이 다를 수 있습니다.
<br>
<br>
그런데 우리는 프로그래밍을 하면서 0과 1로 개발을 한 적이 없습니다. 하지만 컴퓨터는 0과 1만 이해합니다. 우리가 영문으로 작성한 코드를 컴퓨터가 실행하기 위해 우리의 코드를 번역해주는 번역기가 도와주기 때문입니다. 이러한 번역기를 번역 시기에 따라 두가지로 구분하는데 이것이 <span class="emphasis">컴파일러(Compiler)</span>와 <span class="emphasis">인터프리터(Interpreter)</span>입니다.

***

### 컴파일러(Compiler)
컴파일러는 앞서 말했듯 어떤 언어에서 다른 언어로 번역해주는 프로그램을 뜻합니다. 컴파일러는 우리가 작성한 코드를 **한번에 모두 기계어로 번역**합니다. 컴파일러를 실행하면 해당 코드를 모두 번역해서 하나의 바이너리 파일로 저장하고 이후 사용자는 이 바이너리 파일을 실행시킵니다. 즉, 컴파일러 언어를 실행할 때, 코드를 작성하고 컴파일을 한 후 이 컴파일된 파일을 실행해야합니다. 컴파일러는 모든 코드를 번역하기 때문에 번역하는 데에 시간이 오래 걸리고 그 과정이 복잡합니다. 하지만 한번 번역을 하면 바이너리 파일이 생성되어 **메모리에 저장**되며 다음 **실행에 소요되는 시간이 짧아집니다.** 또한 CPU에 따라 명령어 셋이 다를 수 있기 때문에 어떤 컴퓨터에서 컴파일된 코드를 여러 종류의 컴퓨터로 자유롭게 옮겨다니며 사용할 수 없습니다.

***

### 인터프리터(Interpreter)
인터프리터는 컴파일러와 다르게 번역해야할 파일을 받아 **한 줄씩 실행**합니다. 때문에 번역시간은 빠르지만 **실행시간은 느리고**, 바이너리 파일을 만들지 않아 메모리를 사용하지 않습니다. 한번에 모든 코드를 번역하는 컴파일러와 다르게 한 줄씩 번역하여 실행하기 때문에 개발자의 입장에서는 디버깅을 할 때 좀 더 편리하게 사용할 수 있습니다. 인터프리터의 경우 웹 개발에 유용하게 사용할 수 있습니다. 컴파일러 언오로 웹을 개발한다면 오류 코드가 있을 시 전체 페이지가 모두 먹통이 될 수 있습니다. 하지만 인터프리터 언어로 개발한다면 해당 코드를 실행하지 않는 한 에러는 발생하지 않을 수 있습니다. 반면에 효율을 중시하는 분야에서는 컴파일 언어를 사용하여 실행 속도를 최대한 빠르게 하는 것이 중요할 것입니다.
