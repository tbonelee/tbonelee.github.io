---
layout: post
title: "[OS Concepts] 1. Introduction"
date: "2024-01-28 22:01:42 +0900"
categories:
  - 운영체제이론
---
> 'Operating System Concepts \- 10th edition' 을 읽고 정리한
>  내용입니다.


# 1\.1 What Operating Systems Do


- 컴퓨터 시스템을 네 가지로 나누는 관점
	- **hardware**
		- ex) CPU(Central Processing Unit), 메모리, I/O 장치
		- 시스템의 기본적인 연산 리소스 제공
	- **application programs**
		- ex) 스프레드시트, 컴파일러, 웹 브라우저
		- 유저의 컴퓨팅 문제를 풀기 위해 리소스를 어떻게
		 사용할 것인지 정의
	- **user**
	- **operating system**
		- 하드웨어 자체를 조종하고, 다양한 user의 니즈를
		 위한 application program들의 하드웨어 사용을 조정
- 컴퓨터 시스템을 세 가지로 나누는 관점
	- hardware
	- software
	- data
- 운영체제는 다른 프로그램들이 유용한 일을 할 수 있게
 **environment**를 제공하는 역할
	- 그 자체로는 큰 기능을 하는 것이 아니다



 앞으로는 User의 관점과 System의 관점에서 Operating System을
 살펴볼 것이다.
 


## 1\.1\.1 User View


- OS의 목적
	- 많은 컴퓨터 사용자는 랩탑, PC 등을 통해 컴퓨터를
	 사용한다.
	- 이러한 경우 **사용 편의성**, **성능**,
	 **보안**이 목적
	- 유저의 관점에서
	 **자원 활용(resource utilization)** 에는 신경쓰지
	 않는다.


## 1\.1\.2 System View


- OS의 목적
	- **자원 할당(resource allocator)**
	- **control program**


## 1\.1\.3 Defining Operating Systems


- 보편적으로 통용되는 정의는 없다.
	- 다양한 컴퓨터 시스템이 존재하므로
- 보통 "OS는 컴퓨터에서 항상 돌아가고 있는 단 하나의
 프로그램"이라는 정의가 많이 쓰임
	- **Kernel**이 그 하나의 프로그램
	- **System program** : OS와 같이 제공되지만 커널의
	 일부는 아닌 프로그램
	- application programs : 시스템 운영과 상관 없는 모든
	 프로그램
	- 모든 프로그램은 Kernel, system program, application
	 programs 범위 안에 존재
- 오늘날 일반적 목적의 OS들은 **middleware**도 포함
	- 데이터베이스, 멀티미디어, 그래픽 등 애플리케이션
	 개발자의 편의를 위한 추가적 서비스도 제공


# 1\.2 Computer\-System Organization


- 컴퓨터 시스템의 운영
	- 하나 이상의 CPU와 디바이스 컨트롤러가 공유 메모리에
	 대한 접근을 제공하는 **bus**를 통해 연결된 구조
	- CPU와 디바이스는 concurrent하게 실행될 수 있고 메모리
	 사이클을 위해 경쟁한다.
- 디바이스 컨트롤러
	- 특정 디바이스 타입을 책임진다
	- local 버퍼를 가짐
	- OS는 **device driver**를 가지고 이를 통해 디바이스
	 컨트롤러에 대한 접근 가능


## 1\.2\.1 Interrupts


- I/O 작업
	- 디바이스 드라이버가 디바이스 컨트롤러 레지스터에
	 적재함으로써 I/O 명령을 내림
	- 디바이스 컨트롤러가 이를 확인하여 작업 수행
	- 디바이스 드라이버는 OS의 다른 부분으로 제어권을 넘김
	- 디바이스 컨트롤러가 작업을 끝내면 디바이스
	 드라이버에게 작업이 끝났다고 **interrupt**를 통해
	 알리게 된다.
- Interrupt가 발생하면 적절한 Interrupt service routine으로
 제어권이 넘어가야 한다.
	- 보통은 interrupt 정보를 확인하기 위한 일반적인 루틴을
	 호출하면 그 루틴이 특정 interrupt에 해당하는 핸들러를
	 호출한다.
	- interrupt 처리를 빠르게 하기 위해 핸들러 주소는
	 메모리의 낮은 주소에 **interrupt vector**로
	 저장해놓는다
		- 각 인터럽트의 핸들러를 배열 형태로 저장해놓은 형태
		- interrupt에 해당하는 고유 번호를 인덱스로 해서
		 배열에서 찾게 된다.
- Interrupt가 발생하면 interrupt 당한 시점의 상태 정보를
 저장해야 한다.
	- interrupt 서비스로 인해 프로세서의 상태 정보가
	 덮어씌워지더라도 서비스가 마무리되면 다시 기존
	 작업으로 돌아오기 위함


### 기본적인 Interrupt 구현


- CPU 하드웨어에 **interrupt\-request line**라는 장치가
 존재
	- 디바이스 컨트롤러는 interrupt\-request line에 시그널을
	 assert함으로써 interrupt를 *raise*한다.
	- CPU는 interrupt를 *catch*하고 이를 interrupt
	 핸들러로 *dispatch*한다.
	- 핸들러는 디바이스를 서비스하여 interrupt를
	 *clear*한다


### 모던 OS의 Interrupt 구현


- 다음과 같이 보다 복잡한 interrupt 핸들링 피쳐가 보통
 요구된다.
	- 크리티컬한 프로세싱 중에는 인터럽트 핸들링을 미룰 수
	 있어야 한다.
	- 효율적인 방식으로 인터럽트를 디바이스의 적절한
	 핸들러에 디스패치해야 한다.
	- OS가 인터럽트의 우선순위를 구별하여 긴급도에 따라
	 응답할 수 있도록 멀티레벨 인터럽트 체계가 있어야 한다.
- 이러한 기능들은 CPU와
 **interrupt\-controller hardware**를 통해 제공
- How?
	- request line을 두 개로 구분
		- **nonmaskable interrupt**와
		 **maskable interrupt**
		- maskable interrupt line은 CPU가 크리티컬한
		 인스트럭션의 일련을 실행하기 전에 끌 수 있음
	- **interrupt chaining**을 통해 인터럽트 핸들러
	 탐색을 여러 벡터로 분산
		- 보통의 컴퓨터는 인터럽트 벡터의 길이보다 더 많은
		 수의 디바이스를 가지므로 디바이스 핸들러도 벡터의
		 길이보다 많다.
		- 따라서 테이블 요소는 또 다른 인터럽트 핸들러
		 벡터의 주소를 가리킬 수도 있다.
		- 여러 테이블로 분산되면서 dispatch 과정의
		 비효율성이 조금 늘어나긴 했지만 단일 테이블의
		 비효율성을 위해 약간의 타협을 한 것
	- **interrupt priority levels** 시스템을 통해
	 low\-priority 인터럽트를 masking하지 않으면서도
	 high\-priority 인터럽트가 선점되어 실행될 수 있도록
	 해두었다.


## 1\.2\.2 Storage Structure


- CPU는 메모리에서만 인스트럭션을 불러올 수 있다.
	- 따라서 모든 프로그램은 실행을 위해 메모리에 로드되어야
	 한다.
- **RAM(Random\-Access Memory)**
	- rewritable memory
	- main memory
	- 보통 **DRAM(Dynamic Random\-Access Memory)** 으로
	 구현됨
	- **volatile**한 특성 : 전원이 꺼지면 기록이 사라짐
- ROM or EEPROM(Electrically Erasable Programmable Read\-Only
 Memory)
	- 전기적으로 지울 수 있는 메모리
	- **펌웨어(firmware)** 를 ROM으로 동일시하여 부르는
	 경우도 있음
	- 컴퓨터 부팅 시 동작하는 **bootstrap program**을
	 여기에 기록한다.
- 모든 형태의 메모리는 byte의 배열을 제공
	- 각 byte는 고유한 주소를 가진다.
	- 특정 메모리 주소에 `load` 나
	 `store` 인스트럭션을 수행하여 메모리와
	 상호작용
		- `load` : 메인 메모리에서 CPU의 내부
		 레지스터로 byte나 word를 옮기는 인스트럭션
		- `store` : 레지스터에서 메모리로 옮기는
		 인스트럭션
		- 그 외에 CPU는 기본적으로 program counter에 저장된
		 location에서 프로그램을 실행하기 위해 자동적으로
		 메모리에서 인스트럭션을 불러오기도 한다.
- 메인 메모리는 보통 작고 volatile하기 때문에
 **secondary storage**를 둠
	- ex) **HDDs**,
	 **Nonvolatile memory (NVM) devices**
- **tertiary storage**
	- ex) Optical disk, magnetic tapes
	- 백업 카피 보관 등의 목적
- 여러 저장 매체 사이에는 보통 size와 speed 사이의
 trade\-off가 존재
	- 계층적으로 분류할 수 있음
- Volatile / nonvolatile로 구분 가능
- 용어 정리 :
	- **memory** : volatile storage
	- **NVS**(nonvolatile storage)
		- **mechanical** : HDDs, optical disks,
		 holographic storage, magnetic tape, etc.
		- **electrical** : FRAM, NRAM, SSD, etc.
			- **NVM**으로 부르기도 할 것.
- mechanical storage가 보통 electrical storage에 비해 더
 크고 바이트 단위에서 더 저렴하다.
- electrical storage는 보통 mechanical storage에 비해 더
 비싸고 작고 빠르다.


## 1\.2\.3 I/O Structure


- 인터럽트를 통해 I/O를 다루는 것은 데이터 크기가 크면
 무리가 될 수 있다.
	- 매 바이트마다 인터럽트를 통해 cpu를 거쳐야 하는 방식?
- **Direct memory access(DMA)** 를 통해 디바이스에서
 메모리로 큰 데이터를 옮기는 방식을 통해 해결
	- 디바이스 컨트롤러가 I/O 디바이스에 대해 버퍼, 포인터,
	 카운터 등을 세팅하고 디바이스에서 메모리로 데이터 블럭
	 전체를 전송한다(혹은 그 반대 방향으로)
	- CPU의 간섭 x
	- 바이트마다 인터럽트를 발생시키지 않고, 데이터 블럭
	 하나의 전송이 끝났을 때만 디바이스 드라이버에
	 오퍼레이션이 끝났음을 알리는 인터럽트를 발생시킨다.
- 몇몇 하이엔드 시스템은 bus 구조 대신 switch를 사용
	- 컴포넌트간 통신을 공유 bus에서 경쟁하지 않고 스위치
	 상에서 동시적으로 진행할 수 있다.
	- 그렇게 되면 DMA가 더 효율적으로 이루어질 수 있음


# 1\.3 Computer\-System Architecture


## 1\.3\.1 Single\-Processor Systems


- single processing core의 general\-purpose CPU만 존재


## 1\.3\.2 Multiprocessor Systems


- **symmetric multiprocessing**(**SMP**)
	- 모든 프로세서가 모든 작업 동일하게 수행 가능
	- 기존의 싱글코어\-멀티칩 멀티프로세싱 구조 :
		- 하나의 프로세서는 하나의 코어, 하나의 레지스터,
		 하나의 캐시 지님
	- 멀티코어 시스템 :
		- 하나의 프로세서는 여러 개의 (코어, 레지스터, L1
		 캐시), 코어간 공유되는 L2 캐시를 지닐 수 있음
- **non\-uniform memory access** (**NUMA**)
	- 각 CPU (또는 CPU 그룹)에 각자의 로컬 메모리를 제공하여
	 작고 빠른 로컬 bus로 연결하고, CPU끼리는
	 **shared system interconnect**로 연결
	- 기존 방식에서는 스케일이 증가할수록 시스템 bus 접근이
	 집중되어 경합이 발생 \=\> NUMA는 그럴 일이 적음
	- 단점 : 한 CPU가 다른 CPU에 있는 로컬 메모리에
	 접근해야할 때는 성능 이슈가 생길 수 있음. 주의 깊은
	 CPU 스케쥴링이 필요.
- **blade servers**
	- 여러 개의 프로세서 보드, I/O 보드, 네트워크 보드가
	 하나의 섀시에 위치하는 구조
	- 전통적인 멀티프로세서 시스템과의 차이점은 각 blade
	 processor 보드가 독립적으로 부팅되고 각자의 OS를
	 구동한다는 점
	- 각 blade\-server 보드가 멀티프로세서 시스템으로
	 이루어져 있을 수도 있음


## 1\.3\.3 Clustered Systems


- 멀티프로세서 시스템의 일종
- 앞에서 설명한 멀티프로세서와의 차이점은 두 개 이상의
 독립적인 (일반적으로) 멀티코어 시스템으로 구성되어 있다는
 점
	- 느슨하게 결합된 구조
- 보통 고가용성을 위해 사용됨


# 1\.4 Operating\-System Operations


- OS는 프로그램이 실행되는 환경을 제공
- 컴퓨터가 켜지면 펌웨어 내부에 저장된 부트스트랩 프로그램이
 로드되어 실행된다.
	- CPU 레지스터, 디바이스 컨트롤러, 메모리 내용물까지
	 시스템의 모든 측면을 초기화
	- OS 커널의 위치를 찾아 메모리로 커널을 로드
- 커널이 로드되어 실행되면 시스템과 유저를 위한 서비스를
 제공
	- 커널과 별개로 돌아가는 시스템 프로그램에서도 이러한
	 서비스를 제공
		- 시스템 프로그램은 부팅 시간에 메모리에 적재되고
		 커널이 실행될 동안에는 계속 동작한다.
		- ex) 리눅스의 `systemd` : 처음 실행되는
		 시스템 프로그램이고 다른 daemon들을 실행시켜준다.
- 부팅이 끝나고 실행시킬 프로세스, 서비스할 I/O 장치, 응답할
 유저가 없다면 OS는 이벤트가 발생할 때까지 대기
	- 이벤트는 대개 인터럽트의 발생을 통해 시그널을 보낸다.
		- **trap** (or an **exception**) :
		 소프트웨어가 발생시키는 인터럽트 (앞에서는
		 하드웨어가 발생시키는 인터럽트를 살펴봄)
			- 에러로 인해 발생 하거나 (ex. division by zero,
			 invalid memory access)
			- **system call**을 통해 발생
				- system call : 유저 프로그램이 OS 서비스를
				 통해 특정 오퍼레이션을 실행시키기 위한
				 요청1\.4\.1 Multiprogramming and
				 Multitasking
- **Multiprogramming**
	- 하나 이상의 프로그램을 동시에 실행
	- 멀티프로그램 시스템에서 실행중인 프로그램을
	 **process**라고 부름
	- 하나의 프로세스가 I/O 작업과 같이 어떤 작업의 종료까지
	 기다려야 할 때, OS는 다른 프로세스 실행으로 스위치
- **Multitasking**
	- 멀티프로그래밍의 논리적 확장
	- CPU가 여러 개의 프로세스를 아주 빠른 간격으로
	 스위칭하면서 실행하여 유저에게 빠른 응답 시간을 제공
- 메모리에 여러 개의 프로세스 존재 \-\> 메모리 관리의
 필요성
- 실행시킬 프로세스가 여러 개 존재 \-\> 다음 차례에 어떤
 프로세스를 실행시킬지 \- **CPU scheduling**


## 1\.4\.2 Dual\-Mode and Multimode Operation


- **User mode** \& **Kernel mode**(**supervisor mode**, **system mode**, **privileged mode**)
- **Mode bit**를 통해 현재 작업의 모드를 표시 : 커널 (0\)
 or 유저 (1\)
	- 몇몇 위험할 수 있는 인스트럭션은 커널 모드에서만 실행
	 가능하도록 해놓음 : **privileged instructions**
	- 유저 코드가 시스템 콜을 호출하면 하드웨어가 커널
	 모드로 전환시키고, 시스템 콜 return 직전에 유저 모드로
	 전환한다.


## 1\.4\.3 Timer


- 프로세스가 무한 루프에 빠지거나 OS로 영원히 제어권을
 넘기지 않는 상황을 방지
- OS는 카운터를 초기화하고 하나씩 감소시킴
- 카운터가 0이 되면 인터럽트를 발생시킴
- 유저 코드에 제어권을 넘기기 전에 OS는 타이머를 설정하여
 시간이 초과하면 제어권을 가져오거나 프로세스를 종료하도록
 한다.


# 1\.5 Resource Management


## 1\.5\.1 Process Management


- **Process**는 실행 상태의 프로그램
	- 프로그램은 *passive* entity, 프로세스는
	 *active* entity
		- 두 프로세스가 하나의 프로그램 내에서 돌더라도 둘은
		 서로 다른 실행 시퀀스를 가지게 된다.
	- 시스템의 작업 단위
- 프로세스는 작업 완료를 위해 리소스를 필요로 함
	- CPU, 메모리, I/O, 파일
	- 초기 데이터(input)
- 프로세스 종료시 OS는 리소스를 회수해 간다.
- 싱글 스레드 프로세스는 **하나의 program counter**를
 가진다.
	- **program counter** : 다음 실행시킬 인스트럭션의
	 위치를 가지고 있는 레지스터
	- 멀티 스레드 프로세스는 스레드 당 하나의 program
	 counter를 가진다.
- 일반적으로 시스템은 하나 이상의 CPU에서
 **동시에** 돌아가는 프로세스를 가지고 있는다.
	- 몇몇은 유저 프로세스, 나머지는 시스템 프로세스
	- 프로세스/스레드 간에 CPU를 multiplexing하여 동시성을
	 달성


## 1\.5\.2 Memory Management


- 메모리 관리를 통해 메모리 위에 여러 프로그램이 있을 수
 있도록 해준다.
- OS의 메모리 관리 :
	- 메모리 어느 부분이 어느 프로세스에 의해 사용되고
	 있는지 추적
	- 필요한만큼 메모리 공간을 할당/해제
	- 어떤 프로세스(혹은 일부)와 데이터가 메모리에 들어올지
	 혹은 나갈지 결정


## 1\.5\.3 File\-System Management


- **File**
	- 저장 장치의 물리적 속성을 추상화한 것
	- OS는 파일을 물리 매체에 맵핑하고 storage device를 통해
	 파일에 접근한다.
- OS는 파일 관리와 관련하여 다음을 책임진다.
	- 파일 생성/삭제
	- 파일정리를 위해 디렉토리 생성/삭제
	- 파일 및 디렉터리 조작을 위한 기본 요소 지원
	- 파일을 mass storage에 맵핑


## 1\.5\.4 Mass\-Storage Management


- Secondary storge를 통해 메인 메모리를 보조.
- 대부분의 컴퓨터는 HDDs, NVM 장치 등을 프로그램과 데이터의
 주된 저장 장치로 사용한다.
	- 프로그램의 소스이자 목표점
- OS는 Secondary storage 관리와 관련하여 다음을 책임진다.
	- Mounting / Unmounting
	- 유휴 공간 관리
	- 스토리지 할당
	- 디스크 스케쥴링
	- 파티셔닝
	- Protection
- 물론 Tertiary storage도 OS가 관리한다.


## 1\.5\.5 Cache Management


- **Caching** : 느린 저장 공간에 있는 데이터를 임시로 더
 빠른 저장 공간에 저장해서 사용하는 것.
- 하드웨어가 관리하는 캐시도 있고, OS가 관리하는 캐시도
 있음.
- 캐시 사이즈는 제한적이므로 **캐시 관리**가 중요.
	- 캐시 사이즈 선택
	- Replacement 정책
- 멀티프로세서 환경에서는 **캐시 일관성**을 유지하는 것이
 중요
	- 여러 계층에 존재하는 동일 변수에 대한 캐시값이
	 불일치할 수 있음 (한 프로세서에서만 값을 업데이트)
	- 멀티프로세서 환경에서의 캐시 일관성은 보통 하드웨어
	 레벨에서 처리
- 분산 환경에서의 캐시 일관성도 나중에 살펴볼 것


## 1\.5\.6 I/O System Management


- OS는 하드웨어마다 다른 부분들을 **I/O subsytem**뒤로
 숨김으로써 유저가 세세하게 다 알지 않아도 다룰 수 있도록
 해준다.
- I/O subsytem은 여러 컴포넌트로 이루어진다.
	- 버퍼링, 캐싱, 스풀링 등을 포함하는 메모리 관리
	 컴포넌트
	- 일반(general) 장치 드라이버 인터페이스
	- 특정 하드웨어 장치를 위한 드라이버


# 1\.6 Security and Protection


- 다수의 유저, 여러 프로세스의 동시적 실행이 허용되는
 상황에서 데이터 접근 제어는 필수.
- **Protection** : 컴퓨터 자원에 대한 유저, 프로세스의
 접근을 제어하는 메커니즘
- **Security** : 내외부의 공격으로부터 시스템을 보호하는
 것. 몇몇 공격은 OS의 기능으로 여겨지기도 함


# 1\.7 Virtualization


- **Virtualization**
	- 단일 컴퓨터의 하드웨어를 여러 다른 실행 환경으로
	 추상화하는 기술
	- 이를 통해 여러 별개의 환경이 하나의 컴퓨터에서
	 돌아가는 것처럼 보이게 할 수 있음
- **Emulation**
	- 컴퓨터 하드웨어의 동작을 소프트웨어를 통해
	 시뮬레이팅하는 것
	- 목표 CPU 타입이 현재의 CPU 타입과 다를 때 많이
	 사용한다.
		- ex) 애플의 로제타
- 반면에 virtualiztion은 특정 CPU 아키텍쳐에서 돌아가도록
 작성된 OS가 동일 CPU를 타겟으로 하는 다른 OS 위에서
 돌아가게 한다.


# 1\.8 Distributed Systems


- 네트워크로 연결된 컴퓨터 시스템의 집합이 유저에게 시스템
 자원 접근을 제공
- 네트워크 접근을 파일 접근의 형태로 추상화한 OS도 있고,
 네트워크 함수 호출을 통해 접근 가능하도록 한 OS도 있다.
