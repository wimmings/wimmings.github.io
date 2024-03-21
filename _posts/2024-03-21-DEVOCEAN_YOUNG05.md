---
title:  "[따라하며 배우는 AWS 네트워크 입문] 정리 & 실습 과정 - 01장 AWS 인프라" 

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


### 1. AWS 소개

1. 클라우드란 ?  
`인터넷`을 통해 언제 어디서든지 원하는 때 원하는 만큼의 `IT 리소스(서버, 네트워크, 스토리지)`를 손쉽게 사용할 수 있게 하는 서비스

2. AWS는 무엇?  
Amazon Web Services는 전 세계적으로 분포한 데이터 센터에서 다양한 서비스를 제공하고 있는 클라우드 플랫폼으로,  
물리적 데이터 센터와 서버를 구입, 유지 관리하는 대신 AWS로부터  필요에 따라 다양한 기술 서비스에 액세스 할 수 있다.

3. 클라우드 서비스의 종류  
AWS에선 크게 4가지!
- IaaS (Infrastructure as a Service)
       - 기본적인 IT 자원인 `컴퓨팅(EC2), 네트워크(VPC), 스토리지(EBS)` 등을 제공, 운영  <br>
- PaaS (Platform as a Service)  
        - 운영체제와 개발에 필요한 `Middleware, RunTime`을 제공 (**AWS Elastic Beanstalk-애플리케이션 배포**)  <br>
- Serverless  
        - 애플리케이션 개발에 필요한 `대부분`을 클라우드 사업자가 제공, 운영 `(Lambda, API Gateway)`  <br>
- SaaS (Software as a Service)  
        - SaaS는 서비스 공급자에 의해 실행, 관리되는 완전한 제품으로 웹 기반 이메일과 같은 `최종 애플리케이션`을 말한다.

4. AWS 인프라 구조  
<img width="459" alt="1" src="https://github.com/wimmings/wimmings/assets/98014840/39980d2b-d8db-4ee7-ad55-1345f138c937">

### 2. EC2 배포 실습

- EC2(Elastic Compute Cloud) : 물리 환경의 `서버 컴퓨터`와 유사하게 컴퓨팅 리소스를 제공하는 서비스. `가상 머신`으로 제공되며 `인스턴스`라고 부른다.

### [실습 1-1] EC2 배포 및 사용

> 목표 : AWS EC2 인스턴스를 배포 후 해당 인스턴스에 SSH 접속을 하고, 웹 서비스 설치 및 확인하기

<img width="961" alt="2" src="https://github.com/wimmings/wimmings/assets/98014840/f01f6051-61a8-4ad0-8291-f2c13201ef5e">

#### 1. SSH 키 페어 생성하기

> *여기서 잠깐 ! SSH(Secure Shell)란?*

- SSH는 `네트워크`를 통해 `원격 시스템(서버, 장비 등)`에 접근할 수 있는 프로토콜 및 프로그램이다.
- SSH는 **`‘SSH 서버’`** 와 **`‘SSH 클라이언트’`** 로 구성되며, SSH 클라이언트가 SSH 서버에 접속하기 위해선 `인증 절차`가 필요!
    
    <img width="461" alt="3" src="https://github.com/wimmings/wimmings/assets/98014840/f6984485-fd85-4f14-aa43-64f5ad85ca6b">

- 인증 절차는 **`‘키 페어 파일’`** 방식을 사용한다. 
    - `사용자의 개인키`가 노출되지 않게 하고, 
    - `키 파일`로 로그인이 가능해 추가로 암호를 입력하지 않아도 된다.

AWS EC2 인스턴스의 경우에도 기본적으로 SSH 키 페어를 통해 접속한다.
> AWS 가입 후 EC2 > 네트워크 및 보안 > 키 페어 생성

- 윈도우 OS로 PuTTY 프로그램 사용 시  → ppk 선택
- 맥 OS나 리눅스 내장 SSH 사용 시 → pem 선택 ✅

<img width="1073" alt="4" src="https://github.com/wimmings/wimmings/assets/98014840/0a3cff65-e168-4504-90c1-795f4fbfd779">

`.pem 파일`을 잘 보관해 둡니다!

#### 2. AWS 관리 콘솔에서 EC2 인스턴스 배포

| 설정 항목 | 설명 |
| --- | --- |
| AMI(Amazon Machine Image) 선택 | • 인스턴스를 시작하는 데 필요한 소프트웨어 구성(OS, 애플리케이션 서버, 애플리케이션) 이 포함된 템플릿 <br> • Amazon Linux 2 AMI 선택 |
| 인스턴스 유형 선택  | • t2.micro 선택 |
| 인스턴스 세부 구성  | • 네트워크 설정 <br> • ‘퍼블릭 IP 자동 할당’ 활성화 |
| 스토리지 추가  | • 볼륨 크기 30GiB (프리티어 최대 용량) |
| 태그 추가 | • 관리 편의를 위해 서비스를 구분하는 용도 <br> • 키 : Name 값 : devocean_young |
| 보안 그룹 구성 | • 인스턴스에 대한 트래픽을 제어하는 방화벽 규칙 셋 |
| 키 페어 선택 | • 1단계에서 만든 키 페어 선택 |

<img width="1552" alt="5" src="https://github.com/wimmings/wimmings/assets/98014840/8b2573d5-bf4b-491e-8a02-c02a2e4a77b5">

#### 3. 사용자 PC에서 SSH로 EC2 인스턴스 접근

사용자 PC에서 생성된 인스턴스에 SSH 접근하기 위해선 해당 인스턴스의 `퍼블릭 IP` 정보를 확인해야 한다.  
키 파일을 소유자만 볼 수 있도록 권한을 변경한다. 

> `chmod 400 dy_aws.pem`

> `ssh -i dy_aws.pem ec2-user@퍼블릭 IP`

<img width="857" alt="6" src="https://github.com/wimmings/wimmings/assets/98014840/845c534e-fe4d-4a01-8167-bad8a707da59">

#### 4. EC2 인스턴스에 웹 서비스 설치 → 사용자 PC의 웹 브라우저에서 EC2 인스턴스 퍼블릭 IP로 접근

```shell
# 실습의 편리를 위해 root 계정 전환
$ sudo su -

# Web 서비스 설치
$ yum install httpd -y

# Web 서비스 실행
$ systemctl start httpd

# 웹 페이지 구성
$ echo "<h1>Test Web Server</h1>"> /var/www/html/index.html

# curl 명령어로 웹 접속 확인
$ curl localhost
<h1>Test Web Server</h1>
```

웹 브라우저에서 [http://퍼블릭 IP](http://퍼블릭 IP) 로 접근하여 웹 페이지가 정상 출력되는지 확인한다.

<img width="1552" alt="7" src="https://github.com/wimmings/wimmings/assets/98014840/83a4df8b-2851-4520-bd8c-d5a2a46f72a7">

### [실습 1-2] CloudFormation 스택 생성 및 삭제
#### 1. CloudFormation 이란?
자신이 생성할 AWS 인프라 자원을 `코드로 정의하여` 자동으로 자원을 생성하는 방법이다.  
-> 이를 `IaC(Infrastructure as Code)`라 한다.

- 템플릿 : 생성할 AWS 인프라 자원을 `코드`로 정의한 파일 (`JSON` 이나 `YAML` 형식)
- EC2 인스턴스에 대한 `OS, 인스턴스 타입, 키 페어, 태그, 보안 그룹`을 정의하여 배포할 수 O
- 스택 생성 : 템플릿을 CloudFormation에 업로드하여 스택을 생성

#### 2. CloudFormation 스택 생성
> 제공된 yaml 파일을 업로드한 후 임의의 스택 이름을 지정 → 키 페어 선택  
을 마치면 다음과 같이 스택이 생성된다.

<img width="1552" alt="8" src="https://github.com/wimmings/wimmings/assets/98014840/c4e76f14-cbb2-4daf-b431-a1af3318fa2f">

#### 3. 생성된 자원을 확인해 보자!
아까 만든 `devocean_young 인스턴스` 밑에 `WebServer 인스턴스`가 잘 만들어진 것을 볼 수 있다.

<img width="1552" alt="9" src="https://github.com/wimmings/wimmings/assets/98014840/3e425ec1-d272-40b5-83e8-e7a0dc8bb111">

CloudFormation은 처음 사용해보는 서비스였는데 코드로도 다양한 자원을 생성하고 관리할 수 있다는 점이 신기했다.