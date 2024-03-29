#### 어떤 운영체제가 보안적으로 뛰어날까요?

```
리눅스가 윈도우보다 보안적으로 유리한 이유가 몇개 있는 것 같다.

1. 리눅스의 경우 수많은 개발자들이 쓰는 오픈소스이기 때문에 결함이 생기면 빠르게 업데이트가 될 수 있다.
2. 또한 사용자 접근 제어를 엄격하게 관리한다. 최소한의 권한으로 작동하는 원칙을 따르며, 관리자 권한이 없는 일반 사용자는 시스템의 핵심 부분에 접근할 수 없다.
3. 퍼스널 컴퓨터를 포함하면 윈도우 점유율이 훨씬 높기 때문에 해커들의 공격 대상이 되기 쉽다.
```

#### 프로세스와 스레드의 차이

```
프로세스는 컴퓨터에서 실행되고 있는 프로그램을 말한다. 각 프로세스는 독립된 메모리 공간을 할당 받고 명령어들과 데이터를 가진다.

스레드는 프로세스 내에서 실행되는 동작(기능) 단위다. 

스레드는 한 프로세스에서 여러 개의 작업을 동시에 실행 할 수 있다. 

CPU에서 실행되는 단위(unit of execution) 같은 프로세스의 스레드는 컨텍스트 스위칭이 가벼움. 스레드는 자신이 속한 프로세스의 메모리 영역을 공유(스택은 제외)
```

#### 멀티스레드란?

```
스레드는 프로세스 내에서 실행되는 동작(기능) 단위다.

각 스레드는 속해있는 프로세스의 스택 영역을 제외한 나머지 메모리 영역을 공유한다.

멀티 스레드는 하나의 프로세스에서 동시에 여러 작업을 수행하기 위한 것으로 이를 통해 CPU 효율성 증대와 작업속도 증가의 효과가 있다.
```

#### 멀티프로세스와 멀티스레드를 비교해보세요

```
둘 다 동시에 작업을 처리하기 위한 방법이지만 리소스 사용, 컨텍스트 스위칭, 안정성 부분에서 차이가 있다.

리소스: 멀티 프로세스는 독립적인 메모리를 할당 받지만 멀티 스레드는 스택 영역을 제외한 메모리 영역을 공유하기 때문에 리소스 사용이 적다.

컨텍스트 스위칭: 멀티 프로세스는 프로세스 간 통신을 위해 컨텍스트 스위칭이 필요하다. 멀티 스레드는 같은 프로세스 내에서 스레드 간 통신이 일어나기 때문에 컨텍스트 스위칭이 덜 발생한다.

안정성: 스레드는 메모리 영역을 공유하기 때문에 하나의 스레드에서 발생한 문제가 전체 프로세스에 영향을 미칠 수 있다.
```

#### 리눅스 커널 파라미터란?

```
리눅스 운영체제에서 커널에 대한 설정값을 의미. 이 설정값은 시스템 시작 시에 로딩되며, 리눅스 시스템의 동작 방식을 조정하고 최적화하는 데에 사용.

시스템의 보안, 성능, 안정성 등을 조절하는 역할. 대표적인 예로는 TCP/IP 스택의 버퍼 크기, 파일 시스템의 캐시 크기, 메모리 관리 정책 등이 있다.
```

#### 가상메모리에 대해 설명해주세요

![image](https://user-images.githubusercontent.com/28949162/227856956-25ac1a28-68aa-4b81-b889-6cbe124efdbb.png)

```
메모리 관리 기법중의 하나로 프로세스 전체가 메모리에 올라오지 않더라도 실행이 가능하도록 하는 기법. 사용자 프로그램이 물리적 메모리보다 커도 실행이 가능한 장점이 있다. 이때 가상 주소를 논리 주소라 하며 실제 주소는 물리 주소라 한다. 가상 주소는 메모리관리장치(MMU)에 의해 변환되며 이 덕분에 사용자는 실제 주소를 의식할 필요 없이 프로그램을 구축할 수 있다. 가상 메모리는 가상 주소와 실제 주소가 매핑되어 있고 프로세스의 주소 정보가 들어있는 '페이지 테이블'로 관린된다. 이 때, 속도 향상을 위해 TLB를 쓴다.

좋은 프로그램일수록, 자주 사용되지 않는 방어코드나 관리코드가 많다. 자주 사용하지 않는데 물리 메모리에 올려놓는것은 비효율적이다. 이 때문에 가상메모리가 필요하다. 메모리를 논리와 물리로 나눠 필요한 부분만 물리 메모리에 올린다.

Demand Paging: 필요한 부분만 물리 메모리에 올리는 방법.
```

#### 리눅스의 구성요소

```
커널(Kernel)
운영체제의 핵심 역할을 하는 소프트웨어.
하드웨어와 소프트웨어 사이의 인터페이스 역할을 하며, 시스템 자원을 관리하고, 프로세스 스케줄링, 메모리 관리 등을 수행.

쉘(Shell)
사용자와 커널 사이에서 중간 역할을 하는 인터페이스.
사용자가 입력한 명령어를 해석하여 커널에 전달하고, 커널이 반환한 결과를 사용자에게 표시.

파일 시스템(File System)
컴퓨터에 존재하는 파일과 디렉토리 등의 자원을 효율적으로 관리하기 위한 체계.
다양한 파일 시스템이 존재하며, 리눅스에서는 ext2, ext3, ext4, XFS 등이 사용됨.

유틸리티(Utility)
리눅스에서 제공되는 다양한 유틸리티 프로그램으로, 파일 및 디렉토리 관리, 프로세스 관리, 네트워크 설정, 시스템 모니터링 등의 작업을 수행.
```

#### 레드헷 계열과 데비안 계열의 차이? 

```
레드햇 계열의 대표적인 배포판은 RHEL 이다.

RHEL의 특징은 서버용 배포판으로 주로 쓰이고, yum을 사용한 소프트웨어 설치, SELinux를 기반으로 한 기본 보안 기능이 있다.

데비안 계열의 대표적인 배포판은 Ubuntu이다. 주로 개발자나 개인이 선호한다.
```

#### 하이퍼바이저에 대해 설명해주세요 

```
가상화 기술 중 하나로, 하드웨어를 가상화하여 여러 개의 가상 머신(VM)을 생성하고 관리하는 소프트웨어.
```

#### 소켓이란?

```
컴퓨터 네트워크에서 프로세스 간 통신을 위한 도구이며, 프로그래밍에서 네트워크 통신을 하기 위한 인터페이스(API)를 제공합니다. 소켓은 네트워크를 통한 데이터 송수신을 위한 일종의 도어 역할을 하며, 소켓을 통해 서로 다른 컴퓨터끼리 데이터를 주고받을 수 있습니다. 일반적으로 TCP, UDP 등과 같은 프로토콜을 사용하여 데이터를 주고받는 데에 많이 이용됩니다. 소켓은 클라이언트와 서버 간의 통신을 가능하게 하며, 네트워크 프로그래밍에서 매우 중요한 역할을 합니다.
```

#### 운영체제는 메모리를 어떻게 관리합니까? 

```
운영체제는 메모리를 관리하기 위해 가상 메모리와 페이지 교체 기법을 사용합니다.

가상 메모리는 프로세스에 실제로 필요한 메모리 공간만 할당하고, 나머지는 디스크에 저장합니다. 이를 통해 물리적인 메모리 크기보다 더 많은 메모리 공간을 사용할 수 있습니다.

페이지 교체 기법은 물리적인 메모리 공간이 부족할 경우 디스크에 저장된 페이지를 물리적인 메모리 공간으로 가져오는 기법입니다. 대표적인 페이지 교체 알고리즘으로는 LRU(Least Recently Used) 알고리즘이 있습니다. 이 알고리즘은 가장 오랫동안 사용하지 않은 페이지를 먼저 교체합니다. 이외에도 FIFO(First In First Out), Optimal 알고리즘 등이 있습니다.
```

#### 데몬이란 무엇입니까? 

```
백그라운드에서 실행되는 프로그램으로, 일반적으로 사용자 인터페이스를 제공하지 않고 시스템이나 서버 프로세스, 백그라운드 작업 등을 수행하는 프로세스를 가리킨다. 

데몬은 대개 시스템이 부팅될 때 자동으로 실행되어 시스템이나 네트워크를 모니터링하고, 서비스를 제공하며, 자원을 관리하는 등의 역할을 한다.
```

#### 운영체제 스케줄러에 대해 설명해주세요

```
운영체제 스케줄러는 여러 개의 프로세스가 CPU 자원을 사용하고자 할 때, 어떤 프로세스를 우선적으로 처리할지 결정하는 기능을 담당합니다. 이를 통해 CPU 자원의 효율성을 높이고, 프로세스의 처리 시간을 최소화할 수 있습니다.

스케줄러에는 크게 세 가지 종류가 있습니다. 첫 번째로, 장기 스케줄러는 디스크에서 메모리로 프로세스를 가져오는 일을 합니다. 두 번째로, 단기 스케줄러는 메모리 내의 프로세스 중 어떤 프로세스를 우선순위에 따라 CPU에 할당할지 결정합니다. 마지막으로, 중기 스케줄러는 프로세스를 메모리에서 디스크로 스왑아웃하거나, 스왑인하는 역할을 합니다.

스케줄러는 다양한 알고리즘을 사용하여 프로세스를 처리합니다. 대표적인 알고리즘으로는 FCFS(First Come, First Served), SJF(Shortest Job First), RR(Round Robin), Priority Scheduling 등이 있습니다. 이러한 알고리즘들은 프로세스의 우선순위, 작업량, 대기 시간 등을 고려하여 최적의 처리 방법을 선택합니다.
```

#### 커널이란 무엇인가요?

```
운영체제의 핵심적인 부분으로, 시스템 자원 및 하드웨어를 관리하고 응용프로그램과 하드웨어 간의 상호작용을 중재하는 역할을 한다. 

커널은 운영체제의 가장 낮은 레벨에서 동작하며, 프로세스 관리, 메모리 관리, 입출력 관리, 파일 시스템 등 다양한 기능을 수행한다.
```

#### cpu 스케쥴링이 언제 발생하나요?

```

```

#### 페이징의 장점과 단점은?

```
페이징은 가상 메모리 기법 중 하나로, 프로세스의 논리 메모리를 일정한 크기로 분할하여 실제 물리 메모리와 일대일 대응하는 페이지로 매핑하는 기법입니다. 이 때, 물리 메모리가 가용하지 않은 상황에서는 페이지 교체 알고리즘에 따라 페이지를 디스크에 스왑아웃하고 새로운 페이지를 스왑인하는 작업이 발생합니다.

페이징의 주요 장점은 다음과 같습니다.

프로세스의 논리 메모리를 일정한 크기로 분할하여 사용하기 때문에, 물리 메모리가 부족한 상황에서도 프로세스 실행이 가능합니다.
물리 메모리가 일정 크기로 분할되는 것이 아니기 때문에, 프로세스가 필요로 하는 크기의 논리 메모리만 할당하면 됩니다.
물리 메모리의 낭비를 줄일 수 있습니다.

하지만 페이징은 다음과 같은 단점도 가지고 있습니다.

페이지 테이블에 대한 오버헤드가 존재합니다. 페이지 테이블은 매번 참조할 때마다 메모리에서 읽어와야 하므로, 성능에 영향을 미칩니다.
페이지 교체 알고리즘에 따라 페이지 교체 작업에서 디스크 I/O가 발생할 수 있습니다. 디스크 I/O가 많이 발생하면 성능이 저하됩니다.
프로세스가 사용하는 논리 메모리의 크기가 페이지의 크기보다 작은 경우, 낭비되는 메모리가 발생할 수 있습니다.
```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

#### 

```

```

