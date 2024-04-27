---
layout: post
title: Packet Structure
categories: STUDY
modified: 2024-04-22
tags:
  - 네트워크
  - 개발
author:
  name: 두지수 (2mole)
  bio: 개발 및 공부 과정 기록 깃 블로그
  avatar: /images/2mole_profile_img.jpg
  social: Other My Social
comments: true
ads: false
social: true
share: false
image:
  teaser: baekjoon_1.png
---
# Packet Structure
<img src="/images/pcap/networkpacket.jpg" style= "width:400px; margin-left:10px"/>
위 그림은 TCP/IP 네트워크 패킷의 기본을 설명하는 간단한 그림이다.
Level이 의미하는 바는 OSI모델의 계층을 나타낸다.
<img src="/images/pcap/tcpdump-logical-representation.jpg" style= "width:400px; margin-left:10px"/>
위 그림은 기본적인 네트워크 아키텍처의 모습이다.<br>
이제 위 그림들의 패킷을 조금 넓게 펼처본다면 아래의 그림과 같아진다.
<img src="/images/pcap/networkpacket_structer.jpg" style= "width:400px; margin-left:10px"/>
## Ethernet Header
Total Size : 14 byte ( Not include Data, Checksum)
- 목적지 MAC 주소 (Destination Adress) : 6 byte
- 출발지 MAC 주소 (Source Address) : 6 byte
- 프로토콜 타입 (Type) : 2 byte (0x0800 = Ethernet)
- 데이터 (Data) : 46 ~ 1500 byte (IP Header + TCP Header + TCP Data) 
- 체크섬(Checksum, CRC) : 4 byte
	- Ipv4 패킷 헤더의 손상 감지, 헤더 단어를 합산한 16bit 결과를 나타남
	- Ipv6의 경우 Header Checksum을 사용하지 않음

## IP Header
<img src="/images/pcap/ip_header.jpg" style= "width:400px; margin-left:10px"/>
- 버전(Version) : 4 bits
	- 4 bits : IPv4
	- 4 bits : IPv6
- 헤더 길이 정보 (Internet Header Length, IHL) : 4 bit
	- IP Header의 총 길이는 4 bytes 단위로 이루어짐
	- 기본 헤더 길이 : 4 x 5 = 20 bytes
	- 추가 옵션이 주어질 경우 bytes의 길이가 늘어남
- 전송 우선순위(Type of Service, ToS) : 1 byte
	- 데이터 패킷 처리 우선순위
	- Quality of Service(QoS) 관련 설정 정의
- 전체 길이 (Total Length) : 2 byte
	- Ethernet Header를 제외한 전체 패킷의 크기
	- Max Length : 65535 byte
		- 2 byte = 16bit = 2^16 -1 = 65535
- 패킷 ID (Fragment ID) : 2byte
	- 원본 패킷이 여러 개의 단편으로 나뉘었을 때 재조립 할 수 있도록 하는 ID
	- 동일 ID = 같은 패킷의 부분들
- Flags : 3bit 중 2bit 사용됨
	- 예비 byte : 1bit (0)
	- DF(Don't Fragment) : 1bit (단편화 됨 = 0 / 그렇지 않음 = 1)
		- 단편화 : 큰 데이터 패킷을 네트워크를 통해 전송하기 위해 작은 패킷 조각으로 나누는 과정
	- MF(More Fragments) : 1bit (마지막 단편화 데이터 = 0 , 단편화 데이터 더 있음 = 1)
		- 0일 경우 마지막 단편이거나 단편화 되지 않은 패킷을 의미
	- Fragment offset : 13 bits (원본 데이터의 offset)
- TTL(Time to Live) : 1 byte
	- 패킷이 네트워크 내에서 생존 할 수 있는 최대 라우터 수
- 프로토콜(Protocol) : 1 byte 
	- 상위 레벨의 프로토콜 
	- 수신지에서 해당 패킷을 어떤 프로세스 또는 프로토콜 스택에 전달해야 하는지
	- TCP = 0x06 , UDP = 0x11, ICMP = 0x01
- 체크섬(Checksum, CRC) : 2byte
- 출발지 IP 주소 : 4 byte
- 목적지 IP 주소 : 4 byte
- 옵션 : 0 ~ 40
	- IP 헤더의 길이를 증가시킬 수 있는 추가적인 정보를 포함 할 수 있음 , 최대 40바이트까지 확장가능

## TCP Header
<img src="/images/pcap/tcp_header.jpg" style= "width:400px; margin-left:10px"/>
- 출발지 포트 번호 : 2 byte
- 도착지 포트 번호 : 2 byte
- 전송된 순서 (Sequence number) : 4 byte
- 다음 수신 예상 순서(Acknowledge number,ACK)
	- 수신자가 어느 정도의 데이터를 정상적으로 받았는지를 나타냄
-  데이터 위치,헤더 길이(HLEN) : 4bits)
	- 4바이트 단위, 5 = (5 x 4 = 20bytes ) 
-  예비 : 4bits (초기값 : 항상 0)
	- 예비용 : 3bits  
	- NS(Nonce Sum) (1 bit) – ECN-nonce concealment protection 을 위해 사용됨
- Code bits : 1 byte (8 bits)
	- CWR (Congestion Window Reduced)  
	- ECE (ECN-Echo) :  
	- URG( Urgent ) : 긴급 메시지를 전송  
	- ACK( Acknowledgement ) : 수신 양호 표시 
	- PSH( Requests PUSH ) : 버퍼링된 자료 푸쉬용
	- RST( Reset connection ) : 호스트는 즉시 연결을 끊음  
	- SYN( Sync sequence numbers ) :  sequence 번호 동기화  
	- FIN( sender finished ) : 테스트를 거쳐서 연결을 끊음 
- Window Size: 2 byte
	- TCP 흐름제어를 위해 상대편에게 자신의 버퍼 크기를 지속적으로 통보
- Checksum : 2  byte
- Urgent pointer : 2 byte 
	- URG 필드에 의한, 긴급 데이터가 끝나는 위치 정보
- Option : 0~40 byte