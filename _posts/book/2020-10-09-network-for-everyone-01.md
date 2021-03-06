---
title: "모두의 네트워크 #1장: 네트워크 첫걸음"
categories: network-for-everyone
tags: [ network-for-everyone ]
---

# 1장: 네트워크 첫걸음

## 1. 네트워크 구조

- 컴퓨터 간의 연결을 컴퓨터 네트워크라 부른다.
  - 네트워크를 사용하면 컴퓨터 간의 데이터 전송, 웹 사이트 열람, 메일 송 수신을 할 수 있다.
- 인터넷은 전 세계의 큰 네트워크부터 작은 네트워크까지 연결하는 거대한 네트워크다.
- 패킷은 컴퓨터 간의 데이터를 주고받을 때 네트워크를 통해 흘러가는 작은 데이터 조각을 말한다.
- 큰 데이터는 작은 패킷으로 분할한다.
  - 큰 데이터를 그대로 보내면 데이터가 네트워크의 대역폭을 너무 많이 차지(점유)해서 다른 패킷의 흐름을 막을 수 있다.
  - 대역폭은 네트워크에서 이용 가능한 최대 전송 속도로 정보를 전달할 수 있는 단위 시간당 전송량을 말한다.
- 목적지에서 패킷 단위의 데이터를 원래 데이터로 되돌리는 작업을 해야 한다.
  - 패킷은 순서 없이 제각각 도착하지만,
  - 송신 측에서 패킷을 보낼 때 각 패킷에 순서 정보를 붙여 보낸다.
  - 따라서 수신 측에서 원래대로 되돌릴 수 있는 것이다.



## 2. 정보의 양을 나타내는 단위

- 컴퓨터는 0과 1밖에 이해하지 못한다.
  - 디지털 데이터: 0과 1의 집합
- 정보를 나타내는 최소 단위를 비트라고 하며, 비트 8 개를 1바이트라고 한다.
- 숫자와 문자의 대응표를 문자 코드라고 한다.
  - ASCII 코드: 알파벳, 기호, 숫자 등을 다룰 수 있는 기본적인 문자 코드
- 문자 데이터도 사진 데이터도 상대방에서 패킷으로 나누어서 보내면 받은 쪽에서 패킷을 원래 값으로 되돌릴 수 있다.
- 네트워크에 데이터를 전송하는 경우에는 비트 정보를 전기 신호로 변환하기 때문에 실제로 네트워크에 전기 신호가 전송되고 있다. 0과 1의 형태 그대로 전송되는 것이 아니다.



## 3. 랜과 왠

![image](https://user-images.githubusercontent.com/50407047/95562823-a1ec9100-0a57-11eb-8685-ad841efd7064.png)

- 건물 안이나 **특정 지역을 범위로** 하는 네트워크를 랜(Local Area Network, LAN)이라고 한다. 
  - 사무실이나 가정과 같이 지리적으로 제한된 공간에서 컴퓨터나 프린터를 연결할 수 있는 네트워크
  - 좁은 범위를 **랜 케이블을 통해 연결**한다.
- **인터넷 제공자(ISP)가 제공하는 서비스를 사용하여 구축한 네트워크**를 왠(Wide Area Network, WAN)이라고 한다.
- 랜은 왠보다 범위가 좁고 속도가 빠르며 오류가 발생할 확률이 낮다.
- 왠은 랜보다 범위가 넓고 속도가 느리며 오류가 발생할 확률이 높다.

- 인터넷의 시작: 1969년 미국 국방부의 ARPA에서 군사 목적으로 컴퓨터 네 대를 연결한 것을 시작으로 1980년에는 학술 네트워크로, 1990년에는 미국 전역의 네트워크로 확장되었다. 그 후 상용 서비스 업체가 나타나면서 민간에서도 인터넷을 사용할 수 있게 되었다. 



## 4. 가정에서 하는 랜 구성

![image](https://user-images.githubusercontent.com/50407047/95564627-0b6d9f00-0a5a-11eb-94bb-37647feb80db.png)

- 인터넷을 개통할 때는 **인터넷 서비스 제공자(ISP)**와 **인터넷 회선**을 결정하고 계약한다.
  - 인터넷 회선은 광랜을 보통 많이 사용한다.
  - 광랜은 광섬유를 사용하여 고속 통신이 가능하고 전자파의 잡음 영향이 적다.
- **인터넷 서비스 제공자**와 **인터넷 공유기**로 접속한다.
  - 인터넷 공유기(broadband router): 가정이나 소규모 기업에서 인터넷에 접속할 때 쓰인다. 가정용으로 만든 라우터라고 생각하면 된다. 최근에는 라우터의 기능뿐 아니라 허브, 스위칭 허브, 방화벽과 같은 다양한 기능도 제공한다.
- 접속 방식에는 유선 랜 방식과 무선 랜 방식이 있다.
  - 유선으로 연결된 곳을 무선으로 바꿔도 문제가 없다.

> 무선랜카드는 사업자모뎀과 연결할 인터넷선이 없거나 번거로운 장소일 때 공유기를 사용 컴퓨터에 무선랜카드를 장착하여 공유기에 뿌려지는 인터넷 신호를 무선랜카드가 잡아서 인터넷에 연결하는 것이다.  

## 5. (소규모) 회사에서 하는 랜 구성

![image-20201009183850601](C:\Users\Monica Kim\AppData\Roaming\Typora\typora-user-images\image-20201009183850601.png)

![image-20201009183858057](C:\Users\Monica Kim\AppData\Roaming\Typora\typora-user-images\image-20201009183858057.png)

- DMZ는 외부에 서버를 공개하기 위한 네트워크다.

  - 일반 가정에서는 보통 서버를 외부에 공개할 필요가 없고, 하지 않는다. 
  - 따라서 보통 DMZ가 없다.

- 외부에 공개하는 서버에는 주로 웹 서버, DNS 서버, 메일 서버가 있다.

  - 웹 서버: 웹사이트를 공개
  - 메일 서버: 외부 사용자와 메일 주고받기
  - DNS 서버: 외부에서 도메인 이름을 사용하여 회사 서버에 접속

- 회사의 서버는 온프레미스나 클라우드로 운영되고 있다.

  - 온프레미스(on-premise): 사내 또는 데이터 센터에 서버를 두고 운영하는 것

    - 사내에서 서버를 운영하는 경우 회사 내에 서버 장비실을 두고 그곳에 랙(선반)을 설치한다.

    - 랙 안에는 랙에 설치하기 적합한 형태와 크기를 가진 서버, 라우터(보통 무선 랜 기능이 있는 라우터 사용), 스위치를 설치할 수 있다. 

    - 각 서버는 **스위치**와 연결해서 서로 통신할 수 있다.

    - 사무실 안의 컴퓨터와 프린터도 근처에 있는 스위치에 연결하거나 무선 랜 기능을 통해 랜에 연결해야 네트워크를 사용할 수 있다.

      이미지 출처: http://library.gabia.com/contents/infrahosting/2192

      ![rack](http://library.gabia.com/wp-content/uploads/2016/03/1-1.jpg)

    - 데이터 센터: 대량의 데이터를 보관하기 위해 데이터 센터 서버나 네트워크 기기를 설치한 전용 시설

  - 클라우드: 인터넷을 통해 소프트웨어나 하드웨어 등의 컴퓨팅 서비스를 제공하는 것. 따라서 인터넷에 접속하기만 하면 언제 어디서든 이용할 수 있다.

- 각 서버나 컴퓨터는 스위치나 무선 랜 기능을 사용하여 사내 랜에 접속한다.



## 용어정리

- 서버: 컴퓨터 네트워크에서 다른 컴퓨터에 서비스를 제공하기 위한 컴퓨터 또는 프로그램이다. 반대로 서버에서 보내주는 정보 서비스를 받는 측 또는 요구하는 측의 컴퓨터 또는 프로그램은 클라이언트이다. 

- 왠(Wide Area Network, 원거리 통신망): 랜을 다시 하나로 묶는 거대한 네트워크다. 특정 도시, 국가, 대륙과 같이 매우 넓은 범위를 연결하는 네트워크를 말한다. 넓은 지역에 설치된 컴퓨터 간의 정보와 자원을 공유하기에 적합하도록 설계한 컴퓨터 통신망이다.

- 인터넷 서비스 제공자(Internet Service Provider, ISP): 인터넷에 접속하는 수단을 제공하는 주체다. 일반 사용자, 기업체, 기관, 단체 등이 인터넷에 접속하여 인터넷을 이용할 수 있도록 돕는 사업자다.

- DMZ(DeMilitarized Zone): 네트워크 구성 중에서 일반적으로 인터넷인 외부 네트워크와 내부 네트워크 사이에 위치한 중간 지대(서브넷)을 말한다. 네트워크의 보안 영역으로 외부 공격자가 네트워크에 침투하는 것을 막는 역할을 한다.

  이미지 출처: https://m.blog.naver.com/xcripts/70122248114

![DMZ](https://mblogthumb-phinf.pstatic.net/20111025_274/xcripts_1319554710879Nwxz6_JPEG/DMZ%B0%B3%B3%E4%B5%B5.jpg?type=w2)

> 컴퓨터 보안에서의 **비무장지대**(Demilitarized zone, DMZ)는 조직의 내부 [네트워크](https://ko.wikipedia.org/wiki/네트워크)와 (일반적으로 [인터넷](https://ko.wikipedia.org/wiki/인터넷)인) 외부 네트워크 사이에 위치한 서브넷이다. **내부 네트워크와 외부 네트워크가 DMZ로 연결할 수 있도록 허용하면서도, DMZ 내의 컴퓨터는 오직 외부 네트워크에만 연결할 수 있도록 한다**는 점이다. 즉 **DMZ 안에 있는 호스트들은 내부 네트워크로 연결할 수 없다**. 이것은 **DMZ에 있는 호스트들이 외부 네트워크로 서비스를 제공하면서 DMZ 안의 호스트의 침입으로부터 내부 네트워크를 보호**한다. 내부 네트워크로 **불법적 연결을 시도하는 외부 네트워크의 누군가가 있다면, DMZ는 그들에게 막다른 골목**이 된다. DMZ는 일반적으로 메일서버, 웹서버, DNS 서버와 같이 **외부에서 접근되어야 할 필요가 있는 서버들을 위해 사용된다**. 외부 네트워크에서 DMZ로 가는 연결은 일반적으로 [포트 주소 변환](https://ko.wikipedia.org/wiki/포트_주소_변환)(port address translation 또는 PAT)을 통해 제어된다.