# TCP/IP 모델

TCP/IP 모델은 인터넷에서 가장 널리 사용되는 네트워크 프로토콜 모델입니다. 이 모델은 네트워크 통신을 4개의 계층으로 나눕니다: 네트워크 인터페이스 계층, 인터넷 계층, 전송 계층, 응용 계층.

## 1. 네트워크 인터페이스 계층 (Network Interface Layer)

이 계층은 물리적인 네트워크 하드웨어와 직접적으로 관련이 있습니다. 이 계층의 주요 역할은 데이터를 네트워크의 물리적 매체 (예: 이더넷 케이블, Wi-Fi)를 통해 전송하는 것입니다. 이 계층에서 데이터는 프레임(frame)이라는 단위로 패키징됩니다.

## 2. 인터넷 계층 (Internet Layer)

인터넷 계층의 주요 역할은 패킷을 목적지까지 전달하는 것입니다. 이 계층에서는 IP 주소를 사용하여 패킷을 목적지까지 라우팅합니다. 이 계층에서 사용되는 주요 프로토콜은 IP (Internet Protocol), ICMP (Internet Control Message Protocol), ARP (Address Resolution Protocol), RARP (Reverse Address Resolution Protocol) 등입니다.

## 3. 전송 계층 (Transport Layer)

전송 계층의 주요 역할은 데이터의 신뢰성 있는 전송을 보장하는 것입니다. 이 계층에서는 TCP (Transmission Control Protocol) 또는 UDP (User Datagram Protocol)를 사용하여 데이터를 세그먼트(segment) 또는 데이터그램(datagram)으로 패키징하고, 이를 목적지까지 신뢰성 있게 전송합니다. TCP는 신뢰성 있는 전송을 보장하며, UDP는 빠른 전송을 위해 신뢰성을 희생합니다.

## 4. 응용 계층 (Application Layer)

응용 계층은 사용자와 가장 가까운 계층으로, 사용자가 네트워크에 접근할 수 있게 해주는 인터페이스를 제공합니다. 이 계층에서는 HTTP, FTP, SMTP, DNS 등과 같은 고수준 프로토콜이 사용됩니다. 이 계층에서 데이터는 메시지(message)라는 단위로 패키징됩니다.

이렇게 각 계층은 특정한 역할을 수행하며, 각 계층은 그 아래 계층의 서비스를 사용하고, 그 위 계층에 서비스를 제공합니다. 이 모델은 네트워크 통신을 이해하고 설계하는 데 매우 중요한 기본 개념입니다.
