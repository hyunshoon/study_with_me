메트릭 기반 모니터링 시스템. 오픈소스이고 CNCF 프로젝트로 가장 많이 쓰이는 모니터링 툴이다.

## 필요성, 알고 넘어가야하는 용어

#### 모니터링 시스템 필요성

장애 예방과 장애 발생시 빠른 대처와 원인 진단 및 해결을 위해 필요하다.

#### 메트릭

서비스의 상태를 정량적으로 나타내는 데이터. 컨텍스트를 대부분 무시하고 다양한 이벤트에 대해 시간에 따른 집계를 추적한다. 프로메테우스는 메트릭 기반 모니터링 시스템이다.

#### 로깅

특정한 이벤트가 발생하면 그 상황을 서술 기록하는 것이 로그. 텍스트 기반. 메트릭을 보고 문제를 감시했다면 로그를 보고 이를 해결할 수 있다.

## 아키텍처 & 동작 방식

![image](https://user-images.githubusercontent.com/28949162/211866374-83ce23f2-9c06-4c6a-984b-c5fe184a444e.png)

>>
1. 프로메테우스 서버는 서비스 디스커버리(kubernetes_sd, file_sd 등)를 통해 타겟 정보를 불러온다.
2. 타겟으로 부터 메트릭을 스크랩한다. (Pull 방식)
3. 스크랩한 메트릭을 시계열 데이터베이스(TSDB)에 저장한다. 스크랩한 메트릭이 특정 조건을 만족한다면 Alertmanager에 알람을 push 한다.
4. Alertmanager에 알람이 push되면 Alertmanager는 지정된 포인트(예: 슬랙)로 알람을 noti 한다.
5. 그라파나 등 메트릭 시각화 도구는 PromQL을 이용해 메트릭을 시각화 한다.

## 구성요소

## 기능

## 장단점

## 특징



Reference
- https://prometheus.io/docs/introduction/overview/