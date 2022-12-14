# 필요성

컨테이너 오케스트레이션을 위해 필요. 컨테이너 인프라 환경에서 수 많은 컨테이너를 관리하기 위해 필요한 컨테이너 관리 도구중 사실상 표준이다(CNCF 프로젝트).
컨테이너 배포 및 스케줄링, 로드밸런싱 등 오케스트레이션 툴이 없다면 수동으로 제어해야하는 작업들을 자동화 하여 서비스 운영에 도움을 준다.

# 특징

#### 선언적 API

컨테이너 상태를 선언하면 쿠버네티스가 설정된 상태를 맞추는 개념 (예: 레플리카). 선언적이기 때문에 관리가 편리함.

#### 워크로드 분리

# 기능

### 셀프 힐링

에러가 발생한 컨테이너를 재시작하고, 노드가 죽었을 때 컨테이너를 교체하기 위해 다시 스케줄하고, 사용자 정의 상태 체크에 응답하지 않는 컨테이너를 제거하며,
서비스를 제공할 준비가 될 때까지 클라이언트에 해당 컨테이너를 알리지 않는다.

### 스케일링

CPU 사용량에 따라 자동으로 애플리케이션의 스케일을 업 또는 다운한다.

### 서비스 디스커버리와 로드밸런싱

쿠버네티스를 사용하면 익숙하지 않은 서비스 디스커버리 메커니즘을 사용하기 위해 애플리케이션을 수정할 필요가 없다. 
파드에게 고유한 IP 주소와 파드 집합에 대한 단일 DNS 명을 부여하고, 그것들 간에 로드-밸런스를 수행할 수 있다

### 자동화된 롤아웃과 롤백

애플리케이션의 설정 변경시 점진적으로 롤아웃하는 동시에 애플리케이션을 모니터링해서 모든 인스턴스가 동시에 종료되지 않도록 보장한다. 
만약 어떤 문제가 발생하면 쿠버네티스는 변경 사항을 롤백한다.


### 스토리지 오케스트레이션

로컬 저장소, 클라우드 프로바이더 등과 같이 원하는 저장소 시스템을 자동으로 탑재 할 수 있다.

# 동작 원리

![image](https://user-images.githubusercontent.com/28949162/211851524-33e2abcc-65ee-45b4-b94b-06e2b7232f5b.png)

[동작원리 출처](https://kimjingo.tistory.com/188)

쿠버네티스 워크플로우이다. (1, 2 번은 이해를 돕기 위함이지 엄밀히 말하면 쿠버네티스 워크플로우는 아니다.)

1. Docker Image build: 유저의 목적은 컨테이너를 쿠버네티스 플랫폼에 올려 사용하는 것이다. 그렇기에 우선 도커를 이용하여 컨테이너를 빌드한뒤 저장소에 푸시한다.
2. Image Push to Registry: 빌드한 이미지를 도커 허브나 다른 저장소에 push 한다.
3. kubectl create: 유저가 kubectl을 입력
4. kubectl issues REST call: kubectl 명령이 컨트롤 플레인의 API Server에 전달된다.
5. Pod created and scheduled to a worker node: API Server가 스케줄러에 파드 스케줄링을 요청한다. 스케줄러는 노드들의 상태를 보고 적적한 노드를 찾는다.
6. kubelet is notified: 스케줄러가 스케줄링을 선택한 노드의 kubelet에 파드 생성을 요청
7. kubelet instructs Docker to run the image: Pod 생성 요청을 받은 kubelet이 Docker 데몬에게 실제 컨테이너의 생성을 요청. (이 부분은 확실하지 않다. 쿠버네티스 릴리스 버전 1.24부터 Dockershim을 사용하지 않는다. 컨테이너 런타임에 대한 설명은 https://kubernetes.io/ko/docs/setup/production-environment/container-runtimes/ 를 참조하자. **kubelet이 컨테이너 런타임에 컨테이너 컨테이너 생성을 요청**한다는건 바뀌지 않는다.)
8. Docker pulls and runs: 컨테이너 생성 요청을 받은 컨테이너 런타임이 도커 허브나 다른 이미지 저장소에서 이미지를 찾아 이미지를 생성한다. 이렇게 생성된 컨테이너를 쿠버네티스에서는 파드라는 단위로 관리한다.

# 구성 요소(쿠버네티스)

![image](https://user-images.githubusercontent.com/28949162/211334863-93e1ac07-b79d-4a49-bfe2-9131ac56255d.png)



### 컨트롤 플레인 컴포넌트(마스터 노드)

클러스터 관리(스케줄링 등) 클러스터 이벤트 감지 및 반응(예: 레플리카 수 유지)

#### Kube-API-Server

쿠버네티스 API를 노출. ()

#### etcd

클러스터 데이터를 저장하는 key-value 저장소. 백업 필수.

#### kube-scheduler

파드를 노드에 스케줄링. 
스케줄링 결정을 위해서 고려되는 요소는 리소스 요구 사항, 하드웨어/소프트웨어/정책적 제약, 
어피니티(affinity) 및 안티-어피니티(anti-affinity) 명세, 데이터 지역성, 워크로드-간 간섭, 데드라인을 포함.

#### kube-controller-manager

API 서버를 통해 클러스터의 공유된 상태를 감시하고, 현재 상태를 원하는 상태로 이행시키는 컨트롤 루프.

노드 컨트롤러: 노드가 다운되었을 때 통지와 대응에 관한 책임을 가진다.

잡 컨트롤러: 일회성 작업을 나타내는 잡 오브젝트를 감시한 다음, 해당 작업을 완료할 때까지 동작하는 파드를 생성한다.

엔드포인트 컨트롤러: 엔드포인트 오브젝트를 채운다(즉, 서비스와 파드를 연결시킨다.)

서비스 어카운트 & 토큰 컨트롤러: 새로운 네임스페이스에 대한 기본 계정과 API 접근 토큰을 생성한다.

#### cloud-controller-manager
 
클라우드 컨트롤러 매니저를 통해 클러스터를 클라우드 공급자의 API에 연결하고, 해당 클라우드 플랫폼과 상호 작용하는 컴포넌트와 클러스터와만 상호 작용하는 컴포넌트를 구분할 수 있게 해 준다.

### 노드 컴포넌트(워커 노드)

노드 컴포넌트는 동작중인 파드를 유지하고, 쿠버네티스 런타임 환경을 제공

#### Kubelet

클러스터의 각 노드에서 실행되는 에이전트. 파드에서 컨테이너가 확실하게 동작하도록 관리한다.
컨테이너가 파드스펙에 따라 건강하게 동작하게 한다. 

#### Kube-proxy

노드에서 실행되는 네트워크 프록시로, 쿠버네티스 서비스(네트워크 서비스. 파드 집합에서 실행중인 애플리케이션을 노출하는 방법) 개념의 구현부.
노드의 네트워크 규칙을 유지 관리한다. 이 규칙이 내부 네트워크 세션이나 클러스터 바깥에서 파드로의 통신을 가능하게 한다.

#### Container Runtime

컨테이너 실행을 담당하는 소프트웨어

### 애드온

애드온은 쿠버네티스 리소스(데몬셋, 디플로이먼트 등)을 이용하여 클러스터 기능을 구현. 클러스터 단위의 기능을 제공하기 떄문에 애드온에 대한 네임스페이스 리소스는 kube-system 네임스페이스에 속한다.

#### DNS

쿠버네티스 클러스터는 클러스터 DNS를 갖추어야 한다. 클러스터 DNS는 구성환경 내 다른 DNS 서버와 더불어, 쿠버네티스 서비스를 위해 DNS 레코드를 제공해주는 DNS 서버다. 쿠버네티스에 의해 구동되는 컨테이너는 DNS 검색에서 이 DNS 서버를 자동으로 포함한다.

# 쿠버네티스 API

컨트롤플레인의 핵심은 **API 서버**이다. API 서버는 엔드유저, 클러스터, 외부 컴포넌트가 서로 통신할 수 있도록 HTTP API를 제공한다.
쿠버네티스 API를 사용하면 쿠버네티스의 API 오브젝트(파드, 네임스페이스, 컨피그맵, 이벤트)를 쿼리하고 조작할 수 있다.
kubectl을 사용하거나 REST 호출을 사용하여 API에 직접 접근할 수도 있다.
쿠버네티스 API를 사용해 애플리케이션을 작성하는 경우는 클라이언트 라이브러리를 사용하는 것이 좋다.

Reference
- https://kubernetes.io/ko/docs/concepts/overview/
- https://kubernetes.io/ko/docs/concepts/overview/components/
- https://kimjingo.tistory.com/188
