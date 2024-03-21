---
title:  "[따라하며 배우는 AWS 네트워크 입문] 정리 & 실습 과정 - 02장 VPC 기초" 

categories: 
    - DEVOCEAN YOUNG
toc: true
toc_sticky: true
tags: 데보션 데보션영 네트워크

date: 2024-03-21
---

<img width="459" alt="1" src="https://github.com/wimmings/wimmings/assets/98014840/79be68b2-50ab-449d-a5dc-d0b2a1bfbdf1">


📚 [[따라하며 배우는 AWS 네트워크 입문] 책 가이드](https://www.notion.so/AWS-1af579548fd84c268f8f3ee3f26b2ed4?pvs=21)  
위 책에 대한 정리와 실습 과정을 담았습니다.


### 1. VPC

1. VPC(Virtual Private Cloud)란?
    - **독립된 가상의 클라우드 네트워크** 로 사용자가 정의한 `가상 네트워크 상`에서 다양한 AWS 리소스를 실행할 수 있게 지원
    - 사용자는 VPC 내에 `IP 대역, 인터페이스, 서브넷, 라우팅 테이블, 인터넷 게이트웨이, 보안 그룹, 네트워크 ACL` 등을 생성하고 제어 가능
        
        <img width="816" alt="10" src="https://github.com/wimmings/wimmings/assets/98014840/73bcd9cc-af0c-4dd6-a12f-0186d7be0cec">
        
2. VPC 특징

| 특징 | 설명 |
| --- | --- |
| 확장성 | 클라우드 기반으로 손쉽게 VPC 자원 생성 및 삭제, 설정 및 관리의 편의성 제공 |
| 보안 | `인스턴스 레벨`과 `서브넷 레벨`에서 `Inbound` 및 `Outbound` 필터링을 수행할 수 있도록 `보안 그룹`과 `네트워크 ACL` 제공 |
| 사용자 중심 | VPC내 리소스를 사용자가 원하는 대로 손쉽게 제어 가능, 네트워크 지표 및 모니터링 툴을 활용해 높은 가시성 제공 |

### 2. VPC 리소스 소개

#### 1. 서브넷 (Subnet)
1. 네트워크 영역을 부분적으로 나눈 망
2. 서브넷의 IP 대역은 `VPC의 IP 대역`에 속해 있어야 하며, 이미 예약된 IP 주소를 피해 할당해야 한다.
        
    <img width="614" alt="11" src="https://github.com/wimmings/wimmings/assets/98014840/f8bb2697-fd52-4759-8fa3-0eb33c070117">

        
3. public, private 서브넷
    - `public` subnet: public IP를 가지고 **`인터넷 게이트웨이`** 를 통해 외부 인터넷 구간의 사용자와 통신 가능
    - `private` subnet: private IP만 가지고 있어 자체적으로 외부 인터넷 구간의 사용자와 통신 `불가`  
    → **`NAT 게이트웨이`** 를 통해 가능 (밑에 설명)

#### 2. 가상 라우터와 라우팅 테이블
1. VPC를 생성하면 `자동으로` 가상 라우터가 생성! `로컬 네트워크`에 대한 라우팅 경로만 잡혀있음.
2. 아래와 같이 서브넷별로 `새로운 라우팅 테이블`을 생성해 `개별적으로` 매핑시켜줄 수 있음.
    
    <img width="678" alt="12" src="https://github.com/wimmings/wimmings/assets/98014840/6a6c64ba-ec00-42ba-9a46-17b57886dc1c">

    
#### 3. 인터넷 게이트웨이 : VPC에서 인터넷 구간으로 나가는 관문
    
<img width="561" alt="13" src="https://github.com/wimmings/wimmings/assets/98014840/d9e696ae-8075-4803-998c-e33d2c8341d5">

    
#### 4. NAT 게이트웨이
    
<img width="561" alt="14" src="https://github.com/wimmings/wimmings/assets/98014840/4cd3949a-3021-47f0-86ad-cd21105a50b8">
    
#### 5. 보안 그룹과 네트워크 ACL → 자세한 내용은 밑에서

### 3. [실습 2-1] 퍼블릭 서브넷 VPC 구성

> 목표 : 단일 퍼블릭 서브넷 VPC 실습 환경을 구성하고, 이 서브넷에 속한 EC2 인스턴스가 외부 인터넷 구간으로 통신되는 과정 이해

#### 1. VPC 생성

`10.0.0.0/16` 대역의 VPC 를 생성하여 가상의 네트워크 환경을 구성한다.

<img width="1552" alt="15" src="https://github.com/wimmings/wimmings/assets/98014840/bf319fe2-76ba-40fb-85c1-709c34f25635">

이 가상의 네트워크에는 가상의 라우터가 존재하며, `기본 라우팅 테이블`을 활용한다.

#### 2. 퍼블릭 서브넷 생성

생성한 VPC 를 선택한 후, IPv4 CIDR 블록을 `10.0.0.0/24` 로 지정

<img width="1552" alt="17" src="https://github.com/wimmings/wimmings/assets/98014840/f1e130b4-f0cf-4c77-89a7-8c1a0bc0e941">

아직은 가상 라우터가 갖고 있는 `기본 라우팅 테이블`을 사용

#### 3. 인터넷 게이트웨이 생성 및 VPC 연결

`외부` 인터넷 구간과 통신을 하기 위한 `인터넷 게이트웨이`를 생성하고 VPC 와 연결

<img width="1552" alt="18" src="https://github.com/wimmings/wimmings/assets/98014840/7050183c-5da3-40e4-b661-e34568d2e5da">

<img width="615" alt="19" src="https://github.com/wimmings/wimmings/assets/98014840/f3372f8a-6d54-4c7d-b91b-f50ed2c86a48">

연결했지만 인터넷 구간으로 향하는 **라우팅 경로가 없기 때문에**  
외부 인터넷 통신은 `불가능한` 환경 🔥

#### 4. 퍼블릭 라우팅 테이블 생성 및 서브넷 연결

라우팅 테이블은 VPC, 인터넷 및 VPN 연결 내 서브넷 간에 패킷이 전달되는 방법을 지정한다.

<img width="745" alt="20" src="https://github.com/wimmings/wimmings/assets/98014840/2615057b-bfa1-45aa-85fb-f5a8afe6ac01">

`생성한 라우팅 테이블`에 `서브넷을 연결`해주자!


<img width="1552" alt="21" src="https://github.com/wimmings/wimmings/assets/98014840/9fb99b8d-6fdd-40c8-bf30-c18ad6b9e003">

<img width="620" alt="22" src="https://github.com/wimmings/wimmings/assets/98014840/bb56552f-c22d-4da6-b249-78e450062c9d">

이제 퍼블릭 서브넷은 기본 라우팅 테이블이 아닌 `새로 생성한 퍼블릭 라우팅 테이블`을 사용한다.

#### 5. 퍼블릭 라우팅 테이블 경로 추가

현재 퍼블릭 라우팅 테이블에는 외부 인터넷 통신을 위한 `라우팅 경로가 없다` 🔥  
모든 네트워크 (0.0.0.0/0)가 인터넷 게이트웨이로 향하는 라우팅 경로를 추가해주자

<img width="608" alt="23" src="https://github.com/wimmings/wimmings/assets/98014840/d11c516a-c976-423d-84dd-f5fa3bc86794">



> 이제 검증을 해보자!

#### 6. EC2 인스턴스 생성

생성한 VPC와 서브넷을 지정해주자

<img width="1552" alt="24" src="https://github.com/wimmings/wimmings/assets/98014840/9990339c-0bdc-4e7a-ac0b-f93006d50810">

<img width="610" alt="25" src="https://github.com/wimmings/wimmings/assets/98014840/8327e55d-1799-433b-98a7-86e5fa30804d">



#### 7. EC2 인스턴스 접근 후 통신 확인

EC2 인스턴스의 `퍼블릭 IP 주소`를 확인하여 `SSH` 접근을 해보자

<img width="889" alt="26" src="https://github.com/wimmings/wimmings/assets/98014840/33d4b3fa-9056-4782-b1ed-dfe560c5f14a">


```bash
$ ping [google.com](http://google.com)
```

목적지 타깃을 google.com으로! `외부 인터넷으로 정상적인 통신이 가능`