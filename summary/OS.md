운영체제 필수 지식을 정리한다.

[면접을 위한 cs 전공지식노트](http://www.yes24.com/Product/Goods/108887922)를 중심으로 다양한 레퍼런스를 참고하여 정리했다.

운영체제는 사용자가 컴퓨터를 쉽게 다루게 해주는 인터페이스다. 한정된 메모리나 시스템 자원을 효율적으로 분배해준다. 

## 운영체제와 컴퓨터

운영체제의 역할로는 CPU 스케줄링과 프로세스 관리, 메모리 관리, 디스크 파일 관리, I/O 디바이스 관리 등이 있다.

### 운영체제의 역할과 구조

#### 운영체제의 역할

1. CPU 스케줄링과 프로세스 관리: CPU 소유권을 어떤 프로세스에 할당할지, 프로세스의 생성과 삭제, 자원 할당 및 반환을 관리한다.
2. 메모리 관리: 한정된 메모리를 어떤 프로세스에 얼마나 할당할지 관리한다.
3. 디스크 파일관리: 디스크 파일을 어떠한 방법으로 보관할지 관리한다.
4. I/O 디바이스 관리: 마우스와 키보드 등 I/O 디바이스와 컴퓨터간에 데이터를 주고 받는 것을 관리한다.

#### 운영체제의 구조

![](https://velog.velcdn.com/images/hyunshoon/post/2abaf4bc-5a51-44c1-a1c8-7e01b146ea33/image.png)

시스템 콜: 운영체제가 커널에 접근하기 위한 인터페이스이며 유저 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출할 때 쓴다. 프로세스나 스레드에서 운영체제로 어떠한 요청을 할 때 시스템콜이라는 인터페이스와 커널을 거쳐 운영체제에 전달된다. 하나의 추상화 계층이다.
드라이버: 하드웨어를 제어하기 위한 소프트웨어

### 컴퓨터의 요소

컴퓨터는 CPU, DMA 컨트롤러, 메모리, 타이머, 디바이스 컨트롤러 등으로 이루어져 있다. 커널이 프로그램을 메모리에 올려 프로세스로 만들면 CPU가 이를 처리한다.

#### CPU

메모리에 존재하는 명령어를 해석해서 실행하는 일꾼. 산술논리연산장치(논리 연산), 제어장치(Control Unit; 프로세스 조작을 지시하는 CPU 부품), 레지스터(CPU 안에 있는 매우 빠른 임시 기억장치. 메모리보다 훨씬 빠르다)로 이루어져있다.

#### DMA 컨트롤러

I/O 디브사이가 메모리에 직접 접근할 수 있도록 하는 하드웨 장치. CPU 보조 역할.

#### 메모리

데이터나 상태, 명령어 등을 기록하는 장치. CPU는 계산을 담당하고 메모리는 기억을 담당한다. 보통 RAM을 의미한다.

#### 타이머, 디바이스 컨트롤러

## 메모리

CPU는 메모리에 있는 명령어를 처리하는 역할이다.

### 메모리 계층

![](https://velog.velcdn.com/images/hyunshoon/post/b0720227-79aa-4799-b525-a34a5af4eec8/image.png)


#### 캐시

데이터를 미리 복사해 놓은 임시 저장소. 빠른 장치와 느린 장치사이에 오는 속도 차이에 따른 병목 현상을 줄이기 위한 메모리

#### 캐시히트와 캐시미스

캐시에서 원하는 데이터를 찾으면 캐시히트, 찾지 못해 주 메모리로 가서 데이터를 찾으면 캐시미스라 한다.

#### 웹 브라우저의 캐시

대표적인 캐시로는 웹 브라우저의 작은 저장소 쿠키, 로컬 스토리지, 세션 스토리지가 있다. 이러한 것들은 보통 사용자의 커스텀한 정보나 인증 모듈 관련 사항들을 웹 브라우저에 저장해서 추후 서버에 요청할 때 자신을 나타내거나 중복 요청 방지를 위해 쓰인다.

쿠키: 만료기한이 있는 키-값 저장소. 4KB 까지 데이터를 저장할 수 있고 만료기간을 정할 수 있다.(보통 서버가 정한다)

로컬 스토리지: 만료기한이 없는 키-값 저장소. 10MB 까지 저장할 수 있으며 브라우저를 닫아도 유지되고 도메인 단위로 저장, 생성된다.

세션 스토리지: 만료기한이 없는 키-값 저장소. 5MB 까지 저장할 수 있으며 탭 단위로 세션 스토리지를 생성하며 탭을 닫으면 삭제된다.

#### 데이터베이스의 캐싱 계층

데이터베이스를 구축할 때도 메인 데이터베이스 위에 Redis 라는 캐싱 계층을 둬서 성능을 향상 시킨다.

### 메모리 관리

#### 가상 메모리

![](https://velog.velcdn.com/images/hyunshoon/post/0da39fec-d20c-4c3a-a8ec-2f4caf3cc72c/image.png)

![](https://velog.velcdn.com/images/hyunshoon/post/dadb9b84-8cd4-4394-bb6f-611b43760cfa/image.png)

**메모리 관리 기법중의 하나로 프로세스 전체가 메모리에 올라오지 않더라도 실행이 가능하도록 하는 기법.** 사용자 프로그램이 물리적 메모리보다 커도 실행이 가능한 장점이 있다.
이때 가상 주소를 논리 주소라 하며 실제 주소는 물리 주소라 한다. 가상 주소는 메모리관리장치(MMU)에 의해 변환되며 이 덕분에 사용자는 실제 주소를 의식할 필요 없이 프로그램을 구축할 수 있다. 가상 메모리는 가상 주소와 실제 주소가 매핑되어 있고 프로세스의 주소 정보가 들어있는 '페이지 테이블'로 관린된다. 이 때, 속도 향상을 위해 TLB를 쓴다.


좋은 프로그램일수록, 자주 사용되지 않는 방어코드나 관리코드가 많다. 자주 사용하지 않는데 물리 메모리에 올려놓는것은 비효율적이다. 이 때문에 가상메모리가 필요하다. 메모리를 논리와 물리로 나눠 필요한 부분만 물리 메모리에 올린다.

Demand Paging: 필요한 부분만 물리 메모리에 올리는 방법.

![](https://velog.velcdn.com/images/hyunshoon/post/d5aa54ee-65a7-4c56-a716-4f3e76d9aaeb/image.png)



**스와핑:** 가상 메모리에는 존재하지만 실제 메모리에는 없는 데이터나 코드에 접근할 경우 페이지 폴트가 발생한다. 이 때 메모리에서 당장 사용하지 않는 영역을 하드디스크로 옮기고 하드디스크의 일부분을 마치 메모리처럼 불러와 쓰는
것을 스와핑이라고 한다.

#### 스레싱(Thrashing)

메모리의 페이지 폴트율이 높은 것을 의미하며 컴퓨터의 심각한 성능저하를 야기한다. 메모리에 너무 많은 프로세스가 올라가면 스와핑이 많이 일어난다. 그러면 CPU 이용률이 낮아지면서 운영체제는 CPU 이용률이 낮은줄알고 가용성을 높이기 위해 더 많은 프로세스를 올리게 된다. 악순환이 반복된다. 스레싱을 해결하는 방법으로 메모리를 늘리거나 HDD를 SDD로 바꾸거나 작업세트와 PFP가 있다.

#### 메모리 할당

메모리에 프로그램을 할당할 때는 시작 메모리 위치와 메모리의 할당 크기를 기반으로 하는데 연속 할당과 불연속 할당으로 나뉜다.

연속할당: 메모리에 연속적으로 공간을 할당하는 것
- 고정 분할 방식: 메모리를 미리 나누어 관리하는 방식
- 가변 분할 방식: 프로그램의 크기에 맞춰 동적 할당

**불연속 할당**: 현대 운영체제가 쓰는 방식으로 페이징 기법이 있다. 메모리를 4KB 크기의 페이지로 나누고 프로그램마다 페이지 테이블을 두어 이를 통해 메모리에 프로그램을 할당하는 것. 

- **페이징:** 페이징은 동일한 크기의 페이지 단위로 나누어 메모리의 서로 다른 위치에 프로세스를 할당한다. 홀의 크기가 균일하지 않은 문제는 없어지지만 주소 변환이 복잡해진다.
- **세그멘테이션:** 페이지 단위가 아닌 의미단위인 세그먼트로 메모리를 나누는 것. 

#### 페이지 교체 알고리즘

메모리는 한정되어있기 때문에 스와핑이 일어난다. 스와핑은 많이 일어나지 않도록 설계 되어야 하며 이는 페이지 교체 알고리즘을 기반으로 스와핑이 일어난다.


#### 가상 메모리 장점

1. 프로그램이 물리 메모리 제약에서 벗어남
2. 프로그램이 더 작은 메모리를 차지하기 때문에 더 많은 프로세스를 올릴 수 있다.
3. 프로그램을 메모리에 올리고 swap 하는데 필요한 I/O 횟수가 줄어듬

## 프로세스와 스레드

프로세스는 컴퓨터에서 실행되고 있는 프로그램을 말한다.

### 프로세스의 상태

프로세스는 여러가지 상태 값을 가진다.

#### 생성 상태

프로세스가 생성된 상태를 의미하며 fork() 또는 exec() 함수를 통해 생성한다.

#### 대기 상태

대기상태는 메모리 공간이 충분하면 메모리를 할당받고 아니면 대기하고 있으며 CPU 스케줄러로부터 CPU 소유권이 넘어오기를 기다리는 상태이다.

#### 대기 중단 상태, 일시 중단 상태

메모리 부족으로 일시 중단된 상태

#### 실행 상태

CPU 소유권과 메모리를 할당받고 인스트럭션을 수행중인 상태

#### 중단 상태

어떤 이벤트가 발생한 이후 기다리며 프로세스가 차단된 상태. I/O 디바이스에 의한 인터럽션시 자주 발생한다.

#### 종료 상태

종료 상태는 메모리와 CPU 소유권을 모두 놓고 가는 상태. 종료는 자연스럽게 되기도하지만 부모 프로세스가 자식 프로세스를 강제로 종료하는 경우도 있다. 자식 프로세스에 할당된 자원의 한계치를 넘어서거나, 부모 프로세스가 종료되거나, 사용자가 kill 등 명령어로 종료하기도 한다.

### 프로세스의 메모리 구조




Reference
- 주홍철, 면접을 위한 cs 전공지식노트(길벗, 2022)
- https://www.youtube.com/watch?v=5pEDL6c--_k
- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC
- https://math-coding.tistory.com/80
- https://hyeyoo.com/139
- https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/9_VirtualMemory.html