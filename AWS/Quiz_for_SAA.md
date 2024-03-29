1. IaaS, PaaS, SaaS 의 차이점 

`IaaS는 서버나 네트워크 같은 인프라 제공, PaaS는 애플리케이션을 구동할 수 있는 플랫폼을 제공 , SaaS는 애플리케이션을 제공`

2. 리전, 가용영역의 차이점
```
리전 > 가용영역
가용영역: 리전 내에서 물리적으로 격리된 데이터 센터
```
3. 권한, 역할, 정책의 차이점

```
- 정책: 여러 리소스로의 접근 권한을 묶은 것. 하나의 사용자. 장기 또는 영구적 자격. 
- 역할: 그룹 단위에 부여. 역할을 부여받은 모든 대상, 임시 보안 자격 증명
```
4. IAM 보안 모범 사례
```
- 루트 계정 액세스 키 잠금
- MFA 활성화
- 개별 IAM 사용자 생성
- 사용자 그룹을 이용한 접근 권한 할당
```
5. VPC 내부에서 서브넷을 분리하는 목적 두 가지
```
- 역할 분리: 리소스가 담당하는 역할에 따라 분리
- 기기 분리: 내결함성을 높이기 위해. 서브넷 장애를 독립적으로 하기 위해
```
6. 보안 그룹과 NACL의 차이
```
- 보안그룹은 리소스 단위로 적용
- NACL은 서브넷 단위로 적용
```
7. 보안그룹이 무엇을 기준으로 액세스 제어를 하는지?
```
- 포트
- IP
```
8. 점프 서버를 사용하는 이유?

`보안을 위해서. 리소스에 바로 접근하지 않게 하고 서브넷을 분리하여.`


10. 로드밸런서 사용 목적
```
- 부하 분산
- SSL 인증 처리 해줘서 웹서버 부하 낮춤
- 부정 요청 대응
```
11. ALB와 NLB의 차이점
```
- ALB: URL 패턴으로 로드밸런싱 가능. HTTP, HTTPS 처리에 탁월. 더 비쌈
- NLB: IP로 로드밸런싱 가능. 양방향 소켓 통신에 유리
```
12. S3가 EBS보다 우수한 점
```
내결함성 -> S3의 경우 내구성 99.999999%를 달성하도록 설계 되있다고 명시되어있음. 반면에 EBS는 AWS에서 보증하는 수치가 없다.

비용 -> 대략적으로 EBS가 5배 정도 높다. EC2와 함께 사용해야 하므로 EC 인스턴스를 이용하는 비용도 추가 된다.
```

13. S3의 한계

`외부 스토리지. 따라서 EBS와 달리 C 드라이브나 D 드라이브 등으로 활용할 수 엇으며 파일시스템 처럼 마운트 할 수 없다.`

15. 프라이빗 DNS를 사용할 때 주의할 점

`존재하는 퍼블릭 도메인 이름과 중복하지 않도록 주의해야한다.`

------

16. amazone athena란?

```
Amazon Athena는 서버리스 인터랙티브 쿼리 서비스. 이 서비스를 사용하면 사용자는 대용량 데이터를 쉽게 분석할 수 있다.

Athena는 기존의 데이터 웨어하우스와 비교하여 시간과 비용이 더 적게 듭니다. Athena는 데이터를 스키마 형태로 저장할 필요가 없으며, 대신 Amazon S3와 같은 저장소에서 데이터를 쉽게 쿼리할 수 있습니다.
```

17. aws DynamoDB란?

```
완전 관리형 NoSQL 데이터베이스 서비스입니다. DynamoDB는 매우 확장 가능하며, 정확하고 일관된 성능을 제공합니다.

DynamoDB는 기본적으로 키-값 저장소(key-value store)입니다. 이는 매우 간단한 데이터 구조를 가지고 있으며, 기본적으로는 테이블 형식으로 구성됩니다. 각 테이블은 기본 키(primary key)를 사용하여 인덱싱되며, 각 키는 한 개 또는 여러 개의 속성(attribute)으로 구성됩니다. DynamoDB는 유연한 데이터 모델을 지원하여 테이블에 새로운 속성을 추가하거나 제거할 수 있습니다.

또한 DynamoDB는 자동으로 데이터를 분산하고 복제하여 높은 가용성과 내구성을 제공합니다. 이를 통해 빠른 성능, 일관성과 신뢰성을 제공하며, 서버 리소스를 적절하게 사용하여 비용을 절감할 수 있습니다.

DynamoDB는 서버리스(Serverless) 아키텍처와도 함께 사용될 수 있습니다. AWS Lambda와 같은 다른 서비스와 연계하여 사용하면, 더욱 확장성이 높고 유연한 시스템을 구축할 수 있습니다.
```


19. AWS Snowball 이란?

`- Snowball은 데이터를 물리적인 디바이스인 스노우볼 장치에 저장하고, 이 장치를 안전하게 배송하여 AWS 클라우드로 데이터를 전송합니다. 이는 인터넷 대역폭을 통한 데이터 전송이 비용이나 시간 면에서 부적합한 경우에 유용`

20. AWS SNS와 SQS에 대해서

```
AWS SNS는 Simple Notification Service의 약자로, 메시지를 게시하고 구독하는 서비스입니다. 이 서비스는 클라우드에서 사용되는 다양한 애플리케이션, 서비스 및 기타 AWS 서비스 간의 통합을 가능하게 합니다. SNS를 사용하면 특정 이벤트나 알림을 다양한 수신 대상 (이메일, SMS, HTTP/HTTPS 엔드포인트 등)으로 전송할 수 있습니다.

반면, AWS SQS는 Simple Queue Service의 약자로, 분산 애플리케이션 간에 메시지를 전송할 수 있는 관리형 메시지 대기열 서비스입니다. SQS는 애플리케이션이 메시지를 송신하고 다른 애플리케이션이 이를 처리하도록 메시지 대기열을 설정할 수 있습니다. SQS는 분산 애플리케이션에서 발생하는 메시지 처리 문제를 해결하기 위해 설계되었습니다.
```

21. AWS SQS 사용 예시

```
- AWS SQS는 Simple Queue Service의 약자로, 분산 애플리케이션 간에 메시지를 전송할 수 있는 관리형 메시지 대기열 서비스입니다. SQS는 애플리케이션이 메시지를 송신하고 다른 애플리케이션이 이를 처리하도록 메시지 대기열을 설정할 수 있습니다.

- 이메일 큐: 애플리케이션에서 이메일을 전송하는 경우, SQS를 사용하여 이메일 대기열을 만들고 이메일 서비스에서 이를 처리할 수 있습니다. 이렇게 함으로써 이메일 전송 처리가 더욱 안정적이고, 이메일 서비스의 가용성이 높아질 수 있습니다.

- 주문 처리 큐: 전자상거래 웹사이트에서 주문 처리 시스템을 구현하는 경우, SQS를 사용하여 주문 정보를 큐에 넣고 처리될 때까지 대기시킬 수 있습니다. 이를 통해 주문 처리 과정에서 데이터 무결성이 유지되고, 주문 처리 시스템의 확장성이 높아질 수 있습니다.

- 비동기 작업 큐: 일부 작업이 시간이 오래 걸리는 경우, SQS를 사용하여 작업 큐에 넣고 백그라운드에서 처리할 수 있습니다. 이를 통해 사용자는 애플리케이션을 계속 사용할 수 있고, 시간이 오래 걸리는 작업은 비동기적으로 처리됩니다.

- 로그 처리 큐: 로그를 처리하는 데 SQS를 사용할 수 있습니다. 애플리케이션에서 발생하는 로그는 SQS 큐에 넣어 처리하고, 이를 통해 로그 처리 시스템이 더욱 안정적이고 확장 가능해집니다.
```

22. S3 Glacier 란?

```
- AWS에서 제공하는 데이터 아카이브 서비스
- 기본적으로 비용이 낮으며, 데이터 접근 시간이 느리지만 매우 안정적
```
23. AWS Secrets manager란?

`AWS Secrets Manager는 AWS에서 제공하는 완전 관리형 암호 관리 서비스입니다. 이 서비스를 사용하여 암호, API 키, DB 계정 및 기타 암호 값을 안전하게 저장하고 검색할 수 있습니다.`

24. AWS Cloudfront란?

`AWS CloudFront는 AWS에서 제공하는 전 세계적으로 분산된 콘텐츠 전송 네트워크(CDN) 서비스입니다. 이 서비스는 전 세계적으로 분산된 엣지 로케이션에서 콘텐츠를 캐시하고, 사용자가 가장 가까운 엣지 로케이션에서 콘텐츠를 빠르게 전송할 수 있도록 지원합니다.`

25. AWS Aurora란? Aurora의 특징

```
- AWS에서 제공하는 관계형 데이터베이스 관리 시스템(RDBMS). 빠른 성능과 높은 가용성을 제공.
- 높은 성능: Aurora는 고성능 스토리지 계층을 제공하므로 빠른 I/O 성능을 제공합니다. 또한 여러 AZ에 걸쳐 데이터베이스를 복제하여 가용성을 높일 수 있습니다.
- 높은 가용성: Aurora는 여러 AZ에서 자동으로 데이터베이스 복제 및 장애 조치(failover)를 수행하여 가용성을 보장합니다.
- 확장성: Aurora는 자동으로 스케일링되므로, 더 많은 트래픽을 처리할 수 있습니다. 또한 Aurora Replicas를 사용하여 읽기 전용 복제본을 생성하여 읽기 성능을 향상시킬 수 있습니다.
- 관리 편의성: Aurora는 AWS Management Console, AWS CLI, Amazon RDS API 및 다양한 다른 도구를 사용하여 데이터베이스를 관리할 수 있습니다.
- Aurora는 매우 빠르고 가용성이 높으며 확장성이 뛰어나므로, 대규모 웹 애플리케이션, 게임, 광고 플랫폼 등에서 많이 사용됩니다. 또한 MySQL 및 PostgreSQL과의 호환성이 높으므로, 기존의 MySQL 또는 PostgreSQL 데이터베이스를 Aurora로 마이그레이션하는 것도 간단합니다.
```
26. RDS와 Aurora의 차이점

`Amazon Aurora는 더 나은 성능과 확장성을 제공하며, 자동 장애 조치 및 데이터베이스 복제 기능을 갖추어 높은 가용성을 제공합니다. 그러나 비용은 더 높을 수 있습니다.`

27. AWS 보안 서비스들 아는대로

```
- AWS Identity and Access Management(IAM): AWS 계정 및 리소스에 대한 액세스를 관리하는 서비스로, 리소스에 대한 권한을 지정하고 액세스를 제어할 수 있습니다.
- Amazon Virtual Private Cloud(VPC): AWS 클라우드에서 가상의 프라이빗 네트워크를 생성하는 서비스로, 보안 그룹 및 네트워크 액세스 제어 리스트(ACL)를 사용하여 네트워크 보안을 강화할 수 있습니다.
- AWS Key Management Service(KMS): AWS에서 제공하는 고급 암호화 서비스로, 고객 키를 생성, 사용 및 관리하여 데이터를 암호화하고 복호화하는 데 사용됩니다.
- AWS Certificate Manager(ACM): AWS에서 SSL/TLS 인증서를 생성, 관리 및 배포하는 서비스로, 웹 사이트 및 애플리케이션에 대한 보안 연결을 제공합니다.
- AWS WAF(Web Application Firewall): AWS에서 제공하는 웹 애플리케이션 방화벽으로, 악성 트래픽 및 보안 위협을 방지하고 웹 애플리케이션을 보호합니다.
- Amazon GuardDuty: AWS 계정에서 악성 활동을 감지하는 관리형 보안 모니터링 서비스로, 네트워크 활동, AWS API 호출 및 DNS 쿼리를 모니터링하여 보안 위협을 탐지합니다.
- AWS Shield: AWS에서 제공하는 DDoS(분산 서비스 거부 공격) 보호 서비스로, 네트워크 및 애플리케이션 레벨에서 DDoS 공격을 탐지 및 방어합니다.
- AWS Security Hub: AWS 계정의 보안 상태를 관리하는 서비스로, AWS 서비스 및 서드파티 도구에서 보안 이벤트를 모니터링하고 분석하여 보안 위협을 식별합니다.
```

28. AWS Network Firewall 이란?

`AWS Network Firewall은 AWS에서 제공하는 관리형 네트워크 방화벽 서비스입니다. AWS Network Firewall은 VPC(Virtual Private Cloud)에서 네트워크 트래픽을 모니터링하고 제어하는 데 사용됩니다.`

29. AWS Network Firewall 기능

```
- 인바운드 및 아웃바운드 트래픽 필터링: AWS Network Firewall은 VPC 내부 및 외부에서 발생하는 모든 네트워크 트래픽을 모니터링하고, 인바운드 및 아웃바운드 트래픽을 필터링하여 악성 트래픽 및 보안 위협을 탐지하고 차단할 수 있습니다.
- 상태 기반 탐지: AWS Network Firewall은 상태 기반 방화벽 기능을 제공하여 정상적인 연결 상태를 추적하고, 이에 대한 차단 또는 허용 규칙을 적용할 수 있습니다.
- 관리형 서비스: AWS Network Firewall은 관리형 서비스로, AWS가 서비스 구성, 업그레이드 및 유지 보수를 처리하여 비용을 절감할 수 있습니다.
```

30. Amazon QuickSight 란?
```
- 가장 인기 있는 클라우드 네이티브 서비리스 BI 서비스
- Amazon QuickSight에서는 조직의 모든 사람이 자연어로 질문하거나 대화형 대시보드를 통해 탐색하거나 기계 학습을 기반으로 패턴과 이상값을 자동으로 찾는 방법으로 데이터에 대한 이해를 높일 수 있습니다.
```
31. IAM 역할에 권한을 주는것과 유저나 그룹에 권한을 주는것의 차이점
```
- 액세스 방법: 역할에 권한을 부여하면 다른 AWS 계정, IAM 사용자 또는 AWS 서비스와 같은 역할을 가진 다른 AWS 엔터티가 해당 역할을 가지고 권한을 사용할 수 있습니다. 반면에 사용자 또는 그룹에 권한을 부여하면 특정 개인 또는 그룹만이 해당 권한을 사용할 수 있습니다.
- 액세스 기간: 역할에는 일시적으로 권한이 부여될 수 있으며, 이를 통해 AWS 리소스에 대한 일시적인 액세스를 허용할 수 있습니다. 반면에 사용자 또는 그룹에 권한을 부여하면 해당 사용자 또는 그룹이 AWS 계정에서 삭제될 때까지 계속해서 권한을 유지합니다.
- 리소스 액세스: 역할은 AWS 서비스 간의 리소스 액세스를 가능하게 하므로, 서로 다른 AWS 서비스 간에 데이터를 이동하거나 처리할 때 유용합니다. 반면에 사용자 또는 그룹에 권한을 부여하는 경우, 일반적으로 특정 리소스에 대한 액세스를 제한하기 위해 사용됩니다.
```
32. AWS 에서 Gateway LB란?

`AWS Gateway Load Balancer는 VPC에 대한 높은 처리량 및 로드 밸런싱을 지원하는 서비스입니다. 일반적으로 인터넷 게이트웨이 또는 VPN 연결을 통해 VPC로 들어오는 트래픽을 처리합니다.`


33. 캐시 사용시 주의점 두 가지

```
서버에 캐시 데이터 저장 영역 필요
캐시 데이터의 최신화 문제. (시간이 지나 데이터가 바뀔 수 있다)
```

일래스틱 캐시는 기본적으로 key-value 시스템으로 임의의 키에 대해 캐시된 데이터를 반환해준다. 

노드는 일래스틱캐시의 최소 단위. 캐시된 데이터가 실제로 저장되는 영역을 확보한다.

일래스틱 서치에서 샤드를 구성했을 때의 이점

일래스틱 서치에서 클러스터를 구성했을 때의 이점

프라이머리 노드와 복제노드의 차이.
