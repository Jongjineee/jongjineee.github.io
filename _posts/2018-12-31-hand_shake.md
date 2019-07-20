---
layout: post
title: 3 Way Handshake & 4 Way Handshake
description: "3 Way Handshake & 4 Way Handshake"
date: 2018-12-31 
categories: [DataScience]
---

## 3 Way Handshake & 4 Way Handshake
* * *

### 1. 3 Way Handshake

앞선 포스팅에서 용어의 설명을 제외하고 TCP 통신의 과정에 대해 이야기했습니다. <br>
TCP 통신을 위해 네트워크를 연결해야합니다. 이 연결의 과정을 <span class="emphasis">3 Way Handshake</span>라고 부릅니다.

![3way_handshake](https://drive.google.com/uc?id=1vLCxFe3PdepG8PEvbpFsl-hR_8vz8z6N)

그림을 보면 이해하기 쉬울 것 같습니다. <br>
먼저 서버의 포트는 열려있는 상태이며 <span class="emphasis">LISTEN</span> 상태라고 합니다. 클리이언트는 <span class="emphasis">Closed</span> 상태입니다.

<p class="indent">1. 클라이언트에서 서버에 연결 요청을 하기위해 SYN 패킷을 보냅니다.</p>

<p class="indent">2. 서버는 클라이언트의 SYN 패킷을 받고 클라이언트에게 요청을 수락한다는 ACK와 클라이언트도 포트를 열어달라는 SYN 패킷을 전송합니다.</p>

<p class="indent">3. 클라이언트는 서버의 수락 응답을 받고 상태를 ESTABLISHED로 변경합니다. 서버에 요청을 잘 받았다는 ACK를 보냅니다. 클라이언트의 ACK를 받은 서버는 ESTABLISHED로 상태를 변경합니다.</p>

위와 같이 세 번의 통신이 정상적으로 이루어지면, 서로의 포트가 <span class="emphasis">ESTABLISHED</span> 되면서 연결이 됩니다.

* * *

### 1. 4 Way Handshake

이제 연결을 해제하는 방식을 알아보겠습니다. 해제하는 과정은 4 Way Handshake라고 부릅니다. 

![4way_handshake](https://drive.google.com/uc?id=1mVFFOII0tptCEJA5_tXOBtNKBoYbc1LH)

먼저 클라이언트와 서버는 3 Way Handshake로 연결을 하고 데이터를 주고 받은 상태입니다. <span class="emphasis">(ESTABLISHED)</span>

<p class="indent">1. 이후 클라이언트는 연결을 종료하겠다는 FIN 플래그를 전송합니다.</p>

<p class="indent">2. 서버는 클라이언트의 FIN 플래그를 받고 알겠다는 확인 ACK를 보냅니다.</p>

<p class="indent">3. 데이터를 모두 보내고 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송합니다.</p>

<p class="indent">4. 클라이언트는 FIN 메시지를 확인했다는 ACK 를 보냅니다.</p>

<p class="indent">5. 클라이언트의 ACK 메시지를 받은 서버는 소켓 연결을 CLOSE 합니다.</p>

TCP 헤더에는 총 6비트로 되어있는 Code Bit(Flag bit)가 존재합니다. 하나의 비트들이 모두 의미를 갖습니다.
Urg-Ack-Psh-Rst-Syn-Fin 순서로 되어 있으며 해당 위치의 비트가 1이면
해당 패킷은 그 비트의 의미를 갖습니다.
예를들어 SYN 패킷일 경우 000010 이 되고 ACK 패킷은 010000이 됩니다.
