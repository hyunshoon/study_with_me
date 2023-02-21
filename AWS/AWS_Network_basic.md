본 포스팅은 Youtube [AWS Web Services Korea 스타트업을 위한 AWS 핵심 서비스](https://www.youtube.com/watch?v=vCNexbgYmQ8&list=PLORxAVAC5fUWoPJqJxJavUNK_QI8EFWP_&index=3) 를 기반으로 작성하였습니다.
## Goal
```
AWS VPC를 중심으로 AWS 인프라 네트워크 구조 이해
```
# VPC 중심 네트워크 구성하기

Default VPC
보안 강화된 네트워크 위한 VPC 설계 전략
Hybrid Networking

![](https://velog.velcdn.com/images/hyunshoon/post/bfa34a03-960f-4c0c-aaca-e896e464a56c/image.png)

AWS 자원 레벨은 글로벌, 리전, 가용영역으로 이루어져있다.

VPC는 리전 레벨의 자원. 대표적으로 S3도 리전 레벨의 자원이다.

## VPC란?

온프레미스로 구축한다면 데이터 센터에서 물리적으로 네트워크 인프라를 구성해야한다. VPC는 기능은 동일하지만 물리가 아닌 가상의 논리적인 네트워크 인프라를 제공해주는 것.

## Defualt VPC 구성 

### 라우팅 테이블

![](https://velog.velcdn.com/images/hyunshoon/post/8196b238-a922-4799-926b-12a467b7e60a/image.png)


Default VPC는 모두 인터넷 게이트웨이를 가지고 있다. 

라우팅 테이블은 서브넷 레벨의 설정이다. 네 개의 서브넷(서울 리전인 경우 가용영역 네 개)은 하나의 라우팅 테이블에 연결 되어 있다. 

VPC 내부는 라우팅 테이블을 통해 서로 통신이 가능하다. 

그 외 IP 대역은 IGW를 통해 통신한다.

### NACL

![](https://velog.velcdn.com/images/hyunshoon/post/0cedcb23-429c-4e76-b140-f7688a74fccf/image.png)

네트워크 흐름 제어를 위해 NACL을 사용. 상태 비저장 방화벽을 위해 사용한다. 상태 비저장이기 때문에 인바운드와 아웃바운드 모두 설정해야한다. 서브넷 단위의 설정이다. 

기본 설정은 Any to Any 이다. 

Rule 번호를 기준으로 퍼미션의 우선순위가 결정된다. Rule 1에서 막은 것을 Rule 10에서 허용해도 허용 되지 않는다.

### Security Group


![](https://velog.velcdn.com/images/hyunshoon/post/d0aa7df1-718f-4455-a12f-1fc150591ea1/image.png)

보안 그룹은 상태 저장 방화벽이다.


## 보안 강화된 네트워크를 위한 VPC 설계 전략


### CIDR(Classless Inter-Domain Routing) 이란?

클래스 없이 유연하게 네트워크 영역을 나누어 IP 주소를 할당하는 방식

![](https://velog.velcdn.com/images/hyunshoon/post/50c4d8eb-b9a1-4d8c-93d7-cd4cada72b6f/image.png)

### VPC 대역 정하기

- IPv4의 경우 /16 ~ /28 넷마스크 허용
- RFC 1918에 정의된 사설 IP 대역 사용 권장
-  - 10.0.0.0/8: 10.0.0.0 ~10.255.255.255
-  - 172.16.0.0/12: 172.16.0.0 ~ 172.31.255.255
-  - 192.168.0.0/16: 192.168.0.0 ~ 192.168.255.255
- VPC CIDR 변경 불가. 대역 추가는 가능
- 서브넷마다 예약된 IP 고려 필요

### VPC Subnetting

![](https://velog.velcdn.com/images/hyunshoon/post/f1416c87-4d4f-4261-8027-0e873dbe01e5/image.png)

VPC IP 대역으로 할당된 대역을 하나의 라우팅

#### VPC Subnet 모범 사례

![](https://velog.velcdn.com/images/hyunshoon/post/9e602ba8-1130-48e6-b792-ac8f3193c536/image.png)


VPC에서 인터넷으로 노출시킬 표면적을 최소화

서브넷은 라우팅 테이블과 연결하여 네트워크 흐름을 제어할 수 있다.

우선, 외부 통신이 가능한 퍼블릭 서브넷과 프라이빗 서브넷을 분리한다. 퍼블릭 서브넷은 인터넷과 양방향 통신이 가능하므로 필요한 서버를 제외하고는 배치하지 않는 것이 좋다. 이를 통해 VCP에서 배포된 서버들이 인터넷으로 노출되는 것을 최소화 할 수 있다.

### VPC Routing 전략

![image](https://user-images.githubusercontent.com/28949162/220252213-1b9024f6-601d-49f8-8d56-a8def8142b00.png)

인터넷과 양방향 통신이 필요한가?

퍼블릭 서브넷은 VPC 대역외의 모든 IP를 IGW에 라우트 되도록 설정

IGW는 VPC와 외부 인터넷과 통신이 가능하게 해준다.

외부에서는 퍼블릭 IP를 통해 접근할 수 있고 IGW를 통해 패킷이 외부로 나갈 수 있다.

외부 인터넷과 양방향 통신이 필요한지 확인하여 필요하다면 퍼블릭 서브넷에 배포한다.

![image](https://user-images.githubusercontent.com/28949162/220252937-eea55146-41a5-43fa-bf81-32e868df4089.png)


VPC 내부에서만 네트워크 연결이 필요한 경우 프라이빗 서브넷을 생성한다. 

프라이빗 서브넷의 라우트 테이블은 로컬 서브넷만 설정되어있다.

하지만, 프라이빗 서브넷에 배포되는 서버라도 페이먼트 게이트웨이나 서버패치와 같은 작업을 위해 인터넷과 연결이 필요할 수 있다. 

이 때, 인터넷에 접근하기 위해서는 두 가지가 필요하다.

1. NAT 게이트웨이: 퍼블릭 서브넷에 NAT 게이트웨이를 배포. 프라이빗 서브넷에 할당된 라우트 테이블은 로컬 IP 대역 이외에 모든 트래픽은 NAT 게이트웨이를 향하도록 설정하면 된다.
2. 

#### 고가용성 환경을 위해 서버가 다른 가용영역에도 배포된 경우

![image](https://user-images.githubusercontent.com/28949162/220253656-a29fa8df-d9ea-4406-b069-bb7e0ec2287e.png)

1. 다른 가용영역에 있는 NAT 게이트 웨이 활용(b 가용영역에 장애 발생시 아웃바운드 처리 불가능)

![image](https://user-images.githubusercontent.com/28949162/220254055-072bf91d-7a5b-432f-ba06-041dba954f6e.png)

2. NAT 게이트웨이 이중화

![image](https://user-images.githubusercontent.com/28949162/220254233-33a4d7d3-ace9-4b75-9002-90ac23a419f9.png)

RDS 같은 경우는 외부와 통신이 필요 없기 때문에 위와 같이 구성하여 외부와의 통신을 차단.

### VPC NACL 전략

![image](https://user-images.githubusercontent.com/28949162/220254459-4ba2da0d-6ebf-403a-be7f-ffd1b85c0d9a.png)

any to any 설정을 주로 하게 되는데 서브넷 레벨로 특정 IP 대역을 거부할 수 있다.

예를들어 퍼블릭 서브넷에서 DDoS 공격을 받으면 해당 IP를 NACL로 거부하여 공격을 막을 수 있다.

### Security Group 전략

![image](https://user-images.githubusercontent.com/28949162/220255166-632bfd3d-f719-4b98-8bd1-496eab3d2fd5.png)

보안 그룹의 특징은 서로 참조할 수 있다. 예를 들어 EC2 에서 NLB의 보안그룹을 규칙으로 넣을 수 있다.

## Hybrid Networking

![image](https://user-images.githubusercontent.com/28949162/220256013-b174aee6-75f7-41e8-92ab-a4507f603376.png)

다른 AWS 서비스와의 통신.

위와 달리 외부 인터넷을 통하지 않는 방법은? -> AWS 서비스와 연계를 위한 VPC 엔드포인트 타입

- 게이트웨이 엔드포인트
- 인터페이스 엔드포인트(PrivateLink)

#### 게이트웨이 엔드포인트

![image](https://user-images.githubusercontent.com/28949162/220256452-620f3c45-689a-4f46-a612-e45018a526cb.png)

NAT 게이트웨이 사용 대비 장점

- 보안과 비용 이점
- VPC 엔드포인트 Policy를 통해 IAM 정책을 사용하여 접근을 세부적으로 제어 가능

#### PrivateLink

![image](https://user-images.githubusercontent.com/28949162/220257041-1aa02102-8aae-4ba9-a4a7-4090188bf460.png)


게이트웨이 엔드포인트는 S3, DynamoDB만 지원하는데 이 외의 서비스들은 Interface Endpoint를 사용하여 통신하게 된다.

인터페이스 방식은 보안그룹으로 관리할 수 있고 VPC 엔드포인트 Policy를 통해 세부 접근 제어 가능

### 다수의 VCP 사용하는 경우

#### VPC Peering

![image](https://user-images.githubusercontent.com/28949162/220257105-f963d3c8-0f8a-4ebb-b959-0c6b81afc4fb.png)

#### 다수의 VPC를 연결하기 위해서는 1:1 peering을 해야한다.

![image](https://user-images.githubusercontent.com/28949162/220257248-1779e9e0-d491-44f1-bc51-4a0460e1c8ff.png)

위와 같은 경우는 VPC2와 VPC4간의 통신은 불가능하다.

![image](https://user-images.githubusercontent.com/28949162/220257361-b1cf603e-1e67-42f6-849e-5e7780b30fc6.png)


#### AWS Transit Gateway

![image](https://user-images.githubusercontent.com/28949162/220257503-2c5d5745-66c6-41c0-8e72-0d46c8006b81.png)

복잡한 Peering 관계를 제거하여 네트워크를 간소화하여 관리할 수 있다.

#### Data Center와 연동하는 경우: VPN, Direct Connect

![image](https://user-images.githubusercontent.com/28949162/220260378-c9002859-9b05-475c-b276-b6aec47cccf0.png)

Site-to-Site VPN: VPG 터널 두개로 스탠바이, 액티브로 연결.

![image](https://user-images.githubusercontent.com/28949162/220260639-36231043-53d3-4ce1-84b8-5a489a32e63d.png)


Reference

- https://www.youtube.com/watch?v=vCNexbgYmQ8&list=PLORxAVAC5fUWoPJqJxJavUNK_QI8EFWP_&index=3









