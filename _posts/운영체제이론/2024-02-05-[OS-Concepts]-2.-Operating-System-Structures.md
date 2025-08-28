---
layout: post
title: "[OS Concepts] 2. Operating-System Structures"
date: "2024-02-05 00:58:01 +0900"
categories:
  - 운영체제이론
---
> 'Operating System Concepts \- 10th edition' 을 읽고 정리한
>  내용입니다.


# 2\.1 Operating\-System Services


- 유저를 위한 OS 서비스
	- 유저 인터페이스
		- GUI, 터치스크린 인터페이스, CLI
	- 프로그램 실행
		- 프로그램을 메모리로 로드해서 실행
	- I/O 오퍼레이션
		- 보통 효율성과 보호의 이유로 유저가 직접 I/O 장치에
		 접근할 수 없게 되어 있음
		- 따라서 OS가 I/O에 접근할 수 있는 방법을 제공해야
		 함
	- 파일시스템 처리
		- 파일/디렉토리 읽기/쓰기/삭제 등
		- 파일/디렉토리 접근 권한에 따라 접근 제어
	- Communications
		- 프로세스간 통신
			- 동일 컴퓨터 상 or 네트워크를 통해
		- 공유 메모리, 메시지 전달 등의 방법을 사용
	- 에러 탐지
		- CPU, 메모리, I/O 장치, 디스크(디스크, 네트워크,
		 프린터, etc.), 유저 프로그램 등 여러 곳에서의
		 에러를 감지하여 처리
- 시스템 운영의 효율성을 위한 서비스
	- 리소스 할당
		- CPU, 메모리, I/O 장치 등
	- 로깅
		- 자원 사용량 관련 stats, etc.
	- Protection and security
		- 프로세스 간 간섭 방지
		- 시스템 접근 권한이 있는 유저에 대한 인증


# 2\.2 User and Operating\-System Interface


## 2\.2\.1 Command Interpreters


- **shells** : C shell, Bourne\-Again shell, Korn shell,
 etc.
- 커맨드는 쉘 자체의 빌트인 함수로 제공되거나 시스템
 프로그램으로 제공됨.


## 2\.2\.2 Graphical User Interface


- 데스크톱에서 마우스를 사용하여 프로그램 실행, 파일 선택
 등등 작업 수행
- 윈도우즈, KDE, GNOME, etc.


## 2\.2\.3 Touch\-Screen Interface


- 스마트폰, 태블릿 컴퓨터
- 화면 터치를 통해 상호작용


# 2\.3 System Calls


- OS가 제공하는 서비스에 대한 인터페이스를 제공


## 2\.3\.1 Example


## 2\.3\.2 Application Programming Interface


- 보통 프로그램 개발자들은 직접 시스템 콜을 호출하는 대신
 API를 호출 (ex. libc)
	- Portability의 장점
- 그렇지만 많은 시스템 콜 호출 API는 실제 커널 시스템 콜과
 큰 연관성이 있는 형태로 존재
- **System\-Call Interface**
	- 유저의 시스템 콜 호출을 위한 인터페이스 제공
	- 유저 모드에서 시스템 콜 인터페이스 함수를 호출하면
	 커널 모드에서 적절한 구현을 찾아 실행
- OS에 시스템 콜 파라미터를 넘기는 방법
	- 레지스터에 값 설정, 블럭에 값 저장, 스택 메소드, etc.
	- ex) 리눅스의 경우 5개 이하의 파라미터는 레지스터 사용,
	 그 외에는 블럭 메소드 사용


## 2\.3\.3 Types of System Calls


- Process control
	- create process, terminate process
	- load, execute
	- get process attributes, set process attributes
	- wait event, signal event
	- allocate and free memory
- File management
	- create file, delete file
	- open, close
	- read, write, reposition
	- get file attributes, set file attributes
- Device management
	- request device, release device
	- read, write, reposition
	- get device attributes, set device attributes
	- logically attach or detach devices
- Information maintenance
	- get time or date, set time or date
	- get system data, set system data
	- get process, file, or device attributes
	- set process, file, or device attributes
- Communications
	- create, delete communication connection
	- send, receive messages
	- transfer status information
	- attach or detach remote devices
- Protection
	- get file permissions
	- set file permissions


# 2\.4 System Services


- 현대 컴퓨터 시스템 계층에서 하드웨어 \=\> 운영체제 \=\>
 시스템 서비스 \=\> 애플리케이션 프로그램 순서로 계층이
 올라감
- **시스템 서비스** : **시스템 유틸리티**라고도 부름
	- 시스템 콜의 wrapper 역할 혹은 조금 더 복잡한 역할을 함
- 시스템 서비스의 카테고리
	- 파일 관리 :
		- 파일, 디렉토리 생성 / 삭제 / 복사 / 이름 변경 /
		 목록 출력 등의 작업 수행
	- 상태 정보 :
		- 날짜, 메모리 사용량, 사용자수, 성능, 디버깅 정보
		 등을 시스템에 요구
	- 파일 수정 :
		- 파일을 생성/수정하는 텍스트 에디터
	- 프로그래밍 언어 지원 :
		- 보통의 프로그래밍 언어(C, C\+\+, Java, Python)를
		 위한 컴파일러, 어셈블러, 디버거, 인터프리터 등을
		 제공
	- 프로그램 로딩과 실행 :
		- 컴파일된 프로그램을 메모리로 로드하고 실행하기
		 위한 프로그램
		- absolute loaders, relocatable loaders, linkage
		 editors, overlay loaders 제공
		- 연관된 디버깅 시스템 제공
	- 커뮤니케이션 :
		- 프로세스, 유저, 컴퓨터 시스템 간 가상의 커넥션을
		 만드는 메커니즘 제공
		- ex) 웹 브라우징, 이메일 전송, 파일 전송
	- 백그라운드 서비스 :
		- 백그라운드에서 지속적으로 실행되는 시스템 프로그램
		 프로세스를 **services**, **subsystems**,
		 daemons라고 부름.
		- ex) 네트워크 데몬, 프로세스 스케쥴러
		- 중요한 활동을 커널 컨텍스트가 아닌 유저
		 컨텍스트에서 실행하는 OS는 데몬을 활용하기도 함


# 2\.5 Linkers and Loaders


- **Linker**는 **relocatable object file**을 엮어서
 하나의 **executable** 파일을 생성.
	- 링킹 과정에서 다른 오브젝트 파일이나 라이브러리가
	 포함될 수 있음 (ex. standard C)
- **Loader**는 바이너리 실행 파일을 메모리로 로드하여 CPU
 코어 상에서 실행.
- **Relocation**은 프로그램의 부분마다 최종 주소를
 부여하고 코드에 이 주소를 기록하여 코드 실행 과정에서 이를
 호출할 수 있도록 하는 과정
- 몇몇 라이브러리는 실행 파일에 포함하지 않고 relocation
 information만 넣어둠. 대신 프로그램 로딩 과정에서 동적으로
 연결
- ex)
	- `main.c`의 소스 코드
	- `gcc -c main.c`를 통해 컴파일러가
	 `main.o`의 오브젝트 파일 생성
	- `gcc -o main main.o -lm`을 통해
	 `main.o`와 기타 오브젝트 파일을 링크해서
	 실행 파일 `main` 생성
	- `./main` 실행시 로더가 동적으로 링킹되는
	 라이브러리와 함께 실행파일을 메모리에 로드
- 오브젝트 파일과 실행 파일은 보통 스탠다드 포맷이 존재
	- 컴파일된 기계 코드, 프로그램이 참조하는 함수와 변수의
	 메타데이터를 포함하는 심볼 테이블 드응ㄹ 포함
	- 리눅스/유닉스에서는 ELF(**Executable and Linkable Format**)
	- 윈도우즈에서는 **Portable Executable**(PE)
	- MacOS에서는 **Mach\-O**


# 2\.6 Why Applications Are Operating\-System Specific


- 시스템 콜이 OS마다 다르기 때문에 하나의 어플리케이션을
 그냥 다른 운영체제에서 실행할 수 없음.
- 여러 운영체제에서 동일한 애플리케이션을 실행시키기 위한
 방법
	1. 인터프리터 언어로 작성하여 각 OS에서 사용 가능한
	 인터프리터로 실행
	2. 언어의 RTE(Runtime Environment)에 가상 머신을 포함하는
	 언어로 작성 (ex. 자바)
	3. 애플리케이션 개발자가 직접 각 OS에 맞게


# 2\.7 Operating\-System Design and Implementation


## 2\.7\.1 Design Goals


- **user goals**, **system goals**
- 하나의 유일한 정답이 존재하지 않음


## 2\.7\.2 Mechanisms and Policies


- **policy** :
	- *what*의 문제
	- ex) 특정 유저를 위해 타이머가 얼마나 길게 설정될
	 것인가의 문제
- **mechanism** :
	- *how*의 문제
	- ex) 타이머의 구조 자체
- policy와 mechanism의 분리를 통해 유연성 확보
	- 목적에 따라 policy는 자주 바뀔 수 있음


## 2\.7\.3 Implementation


- 어셈블리어부터 더 고급 언어까지 다양하게 사용 가능
- 보통 C, C\+\+로 많이 쓰임
- 고급 언어로 쓸수록,
	- 코드 이해와 디버깅이 쉬워짐
	- 하드웨어간 포팅이 쉬워짐


# 2\.8 Operating\-System Structure


## 2\.8\.1 Monolithic Structure


- 하나의 주소 공간에서 실행되는 단일한 static binary 파일에
 커널 기능을 전부 넣는 구조
- ex) Original Unix OS (커널과 시스템 프로그램 두 파트로
 구성됨)


## 2\.8\.2 Layered Approach


- 시스템을 개별적인 작은 요소로 분리
- 하나의 OS 계층은 추상화 객체의 구현 \- 데이터와 데이터 조작
 오퍼레이션
- 상위 계층이 하위 계층의 함수를 호출
- 구성의 단순함과 디버깅의 용이함이 장점
- 특정 기능 사용을 위해 여러 계층을 뚫고 내려가야할 수도
 있는 단점


## 2\.8\.3 Microkernels


- 커널에서 필수적인 컴포넌트만 빼고 나머지 컴포넌트는 유저
 레벨 프로그램으로 작성해서 다른 주소 공간에서 실행
	- 프로세스 간 커뮤니케이션 / 메모리 관리 / CPU 스케쥴링
- OS를 확장하기 편리하다는 장점
	- 새 서비스 추가는 유저 스페이스에 하면 되기 때문에
	 커널을 수정할 필요가 없다.
- 대부분의 서비스가 유저 모드에서 실행되므로 더 안전하고
 안정적이라는 장점
- ex) Darwin (macOS와 iOS의 커널 컴포넌트)
- 서비스간 통신을 위해 메시지를 서로 다른 주소 공간으로
 복사해야 하기 때문에 성능 저하의 단점


## 2\.8\.4 Modules


- 현재의 많은 OS 시스템 디자인이
 **loadable kernel modules** (**LKMs**)를 사용함.
	- 커널이 코어 컴포넌트들을 가지고 추가적인 서비스는 부팅
	 시간 또는 런타임에 모듈을 통해 연결하는 구조.
- 각 커널 섹션이 정의되고 보호되는 인터페이스를 가진다는
 점에서 레이어 시스템과 유사. 하지만 어떤 모듈이든 다른
 모듈을 호출할 수 있다는 점에서 더 유연한 구조.
- 프라이머리 모듈이 코어 기능과 어떻게 다른 모듈을 불러오고
 그들과 소통할지만 안다는 점에서 마이크로커널과 유사.
 하지만 커뮤니케이션을 위해 메시지 패싱을 invoke할 필요가
 없으므로 더 효율적.


## 2\.8\.5 Hybrid Systems


- 보통은 앞에서 살펴본 구조를 섞어서 사용


### 2\.8\.5\.1 macOS and iOS


- 특징적인 레이어는 다음과 같다
	- User experience layer
		- macOS는 Aqua 유저 인터페이스
		- iOS는 Springboard 유저 인터페이스
	- Application frameworks layer
		- Cocoa, Cocoa Touch 프레임워크
			- Objective\-C와 Swift에 API를 제공
	- Core frameworks
		- Quicktime, OpenGL 등을 포함하는 그래픽/미디어 지원
		 프레임워크를 정의하는 레이어
	- Kernel environment
		- Darwin
			- Mach 마이크로커널과 BSD UNIX 커널을 포함하고
			 있음
- 애플리케이션은 user experience 기능을 사용하거나 이를
 건너뛰어 application framework, core framework, kernel
 environment 를 이용할 수도 있다.
- traps로 알려진 Mach 시스템 콜과 BSD 시스템 콜을 사용 가능
- 시스템 콜 인터페이스 밑에서는 Mach가 주요 OS 서비스를 제공
	- 메모리 관리, CPU 스케쥴링, 프로세스간 커뮤니케이션
	 (IPC)
	- ex) BSD POSIX `fork()`함수 호출시 Mach 가
	 커널 추상화를 통해 프로세스를 표현
- 장치 드라이버와 dynamically loadable 모듈 개발을 위한 I/O
 kit(kernel extensions, or kexts) 제공
- 메시지 패싱으로 인한 성능 저하 이슈에 대응하기 위해
 Darwin에서는 Mach, BSD, I/O kit, 여타 커널 확장을 단일
 주소 스페이스에 둔다.
	- Mach는 pure한 마이크로커널은 아님


### 2\.8\.5\.2 Android


- iOS와 비슷하게 소프트웨어의 계층 스택 구조
- 자바용 안드로이드 API 제공
- 자바 애플리케이션은 ART(Android RunTime)에서 실행될 수
 있는 형태로 컴파일됨
	- 자바 코드 \-\> 자바 바이트코드 (`.class`
	 파일) \-\> 실행 파일 (`.dex` 파일)


# 2\.9 Building and Booting an Operating System


## 2\.9\.1 Operating\-System Generation


- OS를 밑바닥부터 만들려면..
	1. OS 소스 코드 작성
	2. 동작시킬 시스템에 맞게 OS를 설정
	3. OS를 컴파일
	4. OS 설치
	5. 컴퓨터와 OS를 boot


## 2\.9\.2 System Boot


- **booting** : 커널을 불러와서 컴퓨터를 시작하는 과정
	1. **bootstrap program** or **boot loader**가
	 커널의 위치를 찾음
	2. 메모리를 커널로 불러온 후 시작
	3. 커널이 하드웨어를 초기화
	4. 루트 파일 시스템을 마운트


# 2\.10 Operating\-System Debugging


- **debugging** : 시스템의 에러를 찾아서 고치는 행위.
 하드웨어/소프트웨어 모두
	- 성능 문제도 버그로 간주 \-\>
	 **performance tuning**도 디버깅에 포함 :
	 **bottlenecks** 찾아서 없애기


## 2\.10\.1 Failure Analysis


- 프로세스가 fail하면 OS가 에러 정보를 관리자 또는 사용자
 공간에 **로그 파일**로 남김
	- **Core dump**를 남기기도 한다. (프로세스의 메모리를
	 캡쳐한 것)
- **crash** : 커널의 failure
	- crash 발생시 로그 파일에 에러 정보를 기록하고 메모리
	 상태를 **crash dump**로 저장
- OS 디버깅은 프로세스 디버깅과 다른 테크닉이 필요
	- 예를 들어 파일 시스템의 커널 failure가 발생한 경우에
	 상태를 파일 시스템에 기록하고 재부팅하는 것은 위험할
	 수 있다.
	- 이러한 경우를 위해 마련된, 파일 시스템을 포함하고 있지
	 않는 디스크의 한 부분에 커널의 메모리 상태를 기록.
	- 시스템이 재부팅되면 프로세스가 해당 영역에서 데이터를
	 모으고 파일 시스템 상에 crash dump 파일을 기록한다.


## 2\.10\.2 Performance Monitoring and Tuning


### 2\.10\.2\.1 Counters


- 시스템 콜 횟수, 네트워크 장치에 요청된 오퍼레이션 횟수 등
 정량적인 요소를 트래킹
- 보통의 리눅스 시스템은 `/proc` 파일 시스템에서
 지표를 읽어들인다.
	- `/proc`은 프로세스 단위 커널 스탯을
	 쿼리하는 데 사용되는 pseudo 파일 시스템이고, 커널
	 메모리 안에만 존재한다.
	- 프로세스에 부여된 유일한 정수 값의 디렉토리 구조로
	 구성됨
		- ex) `/proc/2155` \-\> 2155 ID의
		 프로세스 스탯
- 윈도우즈에서는 Windows Task Manager 툴 제공


#### Per\-Process


- `ps` : 프로세스의 정보를 보여줌
- `top` : 실시간으로 현재 프로세스들의 지표를
 보여줌


#### System\-Wide


- `vmstat` : 메모리 사용량 지표를 보여줌
- `netstat` : 네트워크 인터페이스 지표를 보여줌
- `iostat` : 디스크의 I/O 사용량 지표를 보여줌


### 2\.10\.2\.3 Tracing


- 카운터 기반 툴이 스탯의 현재 값을 요청하는 반면,
 트레이싱은 특정 이벤트의 데이터를 수집한다.


#### Per\-Process


- `strace` : 프로세스의 시스템 콜 호출에 대한
 트레이스
- `gdb` : 소스 레벨의 디버거


#### System\-Wide


- `perf` : 리눅스 성능 툴의 모음
- `tcpdump` : 네트워크 패킷 수집
