---
layout: post
title: "[OS Concepts] 5. CPU Scheduling"
date: "2024-03-09 21:52:17 +0900"
categories:
  - 운영체제이론
---
> 'Operating System Concepts \- 10th edition' 을 읽고 정리한
>  내용입니다.


# 5\.1 Basic Concepts


- 멀티프로그램의 목적 : CPU utilization의 극대화
- 기본 컨셉
	- CPU에서 어떤 프로세스를 실행
	- 해당 프로세스가 대기해야 하는 순간 도달
	- OS가 프로세스에서 기존 프로세스를 빼고 다른 프로세스를
	 스케쥴해서 실행


## 5\.1\.1 CPU\-I/O Burst Cycle


- 프로세스 실행은 CPU execution의 **cycle**과 I/O 대기로
 이루어짐
- 프로세스 execution은 **CPU burst**로 시작해서
 **I/O burst**와 번갈아가면서 진행됨
- CPU burst의 duration은 아래와 같은 그래프를 보여준다.
 (물론 프로세스, 컴퓨터마다 다르지만 일반적으로..)
	- 짧은 CPU burst가 많고, 긴 CPU burst는 적음
- 이러한 특성을 활용하여 효율적인 CPU\-scheduling 알고리즘을
 짜게 된다.



 !\[\[Pasted image 20240309173530\.png]]
 


## 5\.1\.2 CPU Scheduler


- CPU가 idle 상태가 되면 OS가 ready 큐에서 실행할 프로세스를
 선택해야 한다.
- **CPU scheduler**가 메모리에 올라와 있는 실행할 준비가
 된 프로세스 중에 하나를 선택하고 CPU를 해당 프로세스에
 할당한다.
- 알고리즘에 따라 Ready 큐는 FIFO 큐, 우선순위 큐, 트리,
 unordered 연결 리스트 등 여러 방식으로 구현 가능
- 그렇지만 개념적으로는 CPU 위에서 실행되기 위해 ready 큐에
 프로세스가 줄을 서있는 것으로 보면 됨
- 큐에 올라온 것들은 보통 프로세스의 process control
 blocks(PCBs)이다


## 5\.1\.3 Preemptive and Nonpreemptive Scheduling


- CPU\-scheduling 이 결정을 내려야 하는 네 가지 상황
	1. 프로세스가 running state에서 waiting state로 넘어갈 때
	 (ex. I/O 요청 or 자식 프로세스의 종료를 기다리는
	 `wait()` 호출)
	2. 프로세스가 running state에서 ready state로 넘어갈 때
	 (ex. 인터럽트 발생)
	3. 프로세스가 waiting state에서 ready state로 넘어갈 때
	 (ex. I/O의 완료)
	4. 프로세스가 종료할 때
- 1번과 4번의 경우 스케쥴링에서 딱히 결정할 게 없다
	- ready 큐의 프로세스를 선택해서 실행하면 됨
- 1번, 4번 케이스에만 스케쥴링이 일어나는 경우
 **nonpreemptive** or **cooperative** 라고 한다.
 반대의 경우는 **preemptive**
	- nonpreemptive 스케쥴링에서는 CPU가 프로세스에 할당되면
	 프로세스가 종료되거나 waiting state로 바뀔 때까지 CPU
	 자원을 놓지 않는다
	- 거의 모든 최신 OS는 선점적 스케쥴 알고리즘을 사용
- 선점적 스케쥴링은 데이터를 공유하는 프로세스 사이에 race
 condition을 유발할 수 있다.
- 선점적 스케쥴링을 사용하는 경우 OS 커널 디자인에서도
 고려사항이 많아진다.
	- 커널 데이터를 수정하는 시스템 콜을 진행하던 중에 다른
	 프로세스가 선점되고, 해당 프로세스가 동일한 커널
	 데이터에 접근한다면 예기치 못한 이슈가 발생할 수 있다.


## 5\.1\.5 Dispatcher


- **Dispatcher**는 CPU 스케쥴러가 선택한 프로세스에 CPU
 코어의 제어권을 주는 모듈이다. 이는 다음의 기능을 가짐
	- 하나의 프로세스에서 다른 프로세스로 컨텍스트를 스위치
	- 유저 모드로 스위치
	- 프로그램 재개를 위해 유저 프로그램의 적절한 위치로
	 점프
- **Dispatch latency**
	- 모든 컨텍스트 스위치에서 디스패쳐를 호출
	- 디스패처가 하나의 프로세스를 멈추고 다른 프로세스를
	 시작하는 데 걸리는 시간을 dispatch latency라 부름
		- 실행하던 프로세스의 state를 해당 프로세스의 PCB로
		 저장하고, 실행하려는 프로세스의 PCB에서 state를
		 불러오는 데 걸리는 시간
	- 당연히 빨라야 좋다
- 리눅스에서 컨텍스트 스위치 지표를 확인하는 방법
	- `vmstat` 커맨드를 통해 시스템 전체의
	 컨텍스트 스위치 횟수를 확인
	- `/proc` 파일 시스템을 통해 특정 프로세스의
	 컨텍스트 스위치 횟수를 확인


# 5\.2 Scheduling Criteria


- 스케쥴링 알고리즘을 비교하기 위한 여러 기준들
	- CPU utilization
	- Throughput
	- Turnaround time
	- Waiting time
	- Response time
		- Interactive system에서는 turnaround time보다
		 response time이 중요할 수 있음. 유저에게 빠른
		 응답을 하는 것이 중요하므로
- CPU utilization, throughput 을 극대화하고, turnaround
 time, waiting time, response time을 극소화하는 것이 이상적
- 보통은 average를 최적화하지만 종종 min, max 값을
 최적화하는 것이 나을 때도 존재
	- ex) 유저 경험을 좋게 하기 위해 최대 response time을
	 최소화
- 인터랙티브한 시스템(PC, 모바일)에서는 response time의
 분산을 최소화하는 것이 평균을 줄이는 것보다 중요하다는
 주장도 있음
- 이번 장의 예시는 프로세스 당 하나의 CPU burst, 평균
 waiting time의 지표로 단순화하여 살펴 본다.


# 5\.3 Scheduling Algorithms


- 싱글 프로세싱 코어의 단일 CPU로 단순화하여 살펴봄


## 5\.3\.1 First\-Come, First\-served Scheduling (FCFS)


- 가장 먼저 요청한 순서대로 자원을 받아감
- FIFO 큐로 간단히 구현 가능
	- 프로세스가 ready 큐에 들어오면 큐의 tail에 PCB가
	 연결됨
	- CPU가 사용 가능해지면 큐의 head 프로세스가 할당됨
		- 실행되는 프로세스는 큐에서 제거됨
- 평균 waiting time이 길어지는 단점
	- CPU burst time이 긴 프로세스가 긴 프로세스가 먼저
	 할당되면 그 뒤의 금방 끝날 수 있는 프로세스들도
	 꼼짝없이 기다려야 한다.
	- ex)
		- P1 \- 24ms, P2 \- 3ms, P3 \- 3ms 소요
		- P1 \-\> P2 \-\> P3 순서로 실행되면 총 대기시간 :
			- P1 \- 0ms, P2 \- 24ms, P3 \- 27ms \=\> 51ms
		- P2 \-\> P3 \-\> P1 순서로 실행되면 총 대기시간 :
			- P1 \- 6ms, P2 \- 3ms, P3 \- 0ms \=\> 9ms
	- 프로세스의 CPU burst time 분포가 넓은 경우 평균 대기
	 시간이 쉽게 요동칠 수 있다.
- **Convoy effect** : 큰 하나의 프로세스가 CPU 작업을
 마칠 때까지 나머지 작업을 기다리는 현상
- 각 프로세스가 일정한 간격으로 CPU를 점유해야 하는
 인터랙티브한 시스템에서 매우 불리


## 5\.3\.2 Shortest\-Job\-First Scheduling (SJF)


- 다음 CPU burst가 가장 짧은 프로세스를 할당. 동률인 경우
 먼저 요청한 프로세스를 할당
	- total CPU burst의 길이가 아님
- 주어진 프로세스에 대해 평균 대기 시간을 최소화할 수 있음
- 현실에서는 프로세스의 다음 CPU burst 길이를 알 수 없음
	- approximation을 사용
	- 보통 측정된 과거 CPU burst 길이의
	 **exponential average** 을 활용
		- $\tau_{n+1} = \alpha t_{n} + (1-\alpha) \tau_{n}$
		 for $0 \leq \alpha \leq 1$
		- $n$기에 $\tau_{n+1}$ 을 예측할 때 관측된 정보
		 $t_{n}$과 이전의 예측 정보 $\tau_{n}$을 활용
		- 보통 $\alpha = 1/2$로 둘의 가중치를 같게 둠
		- 초기값 $\tau_{0}$은 상수나 전체 시스템 평균으로
		 정의한다.
		- 식을 연쇄적으로 대입하여 전개하면 $\tau_{n+1} =
                              \sum_{i = 0}^{n} (1 - \alpha)^{i} \alpha t_{n- i}
                              + (1 - \alpha)^{n+1} \tau_{0}$
- Preemptive / nonpreemptive 모두 가능
	- ready 큐에 새 프로세스가 들어왔을 때 새 프로세스의
	 다음 CPU burst가 현재 진행중인 프로세스의 남은 CPU
	 burst보다 짧을 때 선택의 기로에 놓인다.
	- Preemptive SJF 알고리즘은 기존 프로세스 대신 새
	 프로세스를 할당
	- Nonpreemptive SJF 알고리즘은 기존 실행중인 프로세스가
	 CPU burst를 마칠 때까지 기다린다
	- Preemptive SJF 스케쥴링을
	 **shortest\-remaining\-time\-first** 스케쥴링으로
	 부르기도 한다.


## 5\.3\.3 Round\-Robin shceduling (RR)


- FCFS 스케쥴링과 비슷하지만 시스템이 프로세스 스위치를 할
 수 있게 선점 기능을 추가한 알고리즘
- **time quantum** or **time slice**라는 시간의 작은
 단위를 정의
	- 보통 10 \~ 100ms
- Ready 큐는 circular 큐로 간주됨
- CPU 스케쥴러가 ready 큐를 돌면서 각 프로세스에 1 time
 quantum 인터벌만큼의 시간을 할당
- Ready 큐를 프로세스의 FIFO 큐로 간주
- 어떤 프로세스도 1 time quantum보다 많은 시간을 연속하여
 할당받을 수 없음
	- 어떤 프로세스의 CPU burst가 1 time quantaum 을
	 초과하면 프로세스는 다른 프로세스에 의해 선점되므로
	 선점적 알고리즘으로 볼 수 있음
- RR 알고리즘의 성능은 time quantum의 사이즈에 크게 의존
	- time quantum을 극단적으로 늘리면 RR 정책은 FCFS 정책과
	 같아진다.
	- time quantum을 아주 작게 하면 수많은 컨텍스트 스위치가
	 발생
		- 예를 들어 하나의 프로세스만 실행하면 될 때 CPU
		 burst보다 time quantum이 작게 되면 불필요한
		 컨텍스트 스위치가 발생
	- 따라서 컨텍스트 스위치 시간 대비 time quantum이 충분히
	 큰 것이 유리
		- 컨텍스트 스위치 시간이 time quantum의 10% 정도
		 비율이면 CPU time의 10% 정도가 컨텍스트 스위칭에
		 사용되게 된다.
	- 실제로 대부분의 최신 시스템은 10 \~ 100ms의 time
	 quantum을 가지고 있음. 컨텍스트 스위칭에 필요한 시간은
	 보통 10ms 미만이므로 컨텍스트 스위칭 시간은 time
	 quantum의 아주 작은 부분
- Turnaround time도 time quantum 사이즈에 영향을 받는다.
	- 프로세스가 하나의 time quantum 안에 다음 CPU burst를
	 끝내는 경우가 많을수록 turnaround time이 줄어드는 경향
		- 한 번에 일을 못 끝내고 중간중간 쉬어가기 때문 \+
		 컨텍스트 스위칭 시간
- 경험칙으로는 80%의 CPU burst가 time quantum보다 짧도록
 설정하는 것이 좋다고 한다.


## 5\.3\.4 Priority\-Scheduling


- 각 프로세스마다 우선순위가 있고 가장 높은 우선순위의
 프로세스에 CPU를 할당. 동일한 우선순위는 FCFS 순서로
 스케쥴됨
	- SJF 알고리즘도 priority\-scheduling 알고리즘의 특수한
	 케이스
		- 우선순위 $p$가 다음 CPU burst의 역수인 알고리즘
- 우선순위는 내부적으로도 외부적으로도 정의될 수 있음
	- 내부적으로 정의되는 우선순위는 time limit, 메모리
	 요구량, open 파일의 갯수, 평균 CPU burst 대비 평균 I/O
	 burst 등 측정 가능한 양적 요소
	- 외부적으로 정의되는 우선순위는 OS 밖에서 설정된 기준.
	 사용되는 컴퓨터에 지불된 기금의 종류와 양, 작업을
	 지원하는 부서 등 여러 요소가 될 수 있음
- Preemptive / Nonpreemptive 모두 가능
- **Indefinite blocking**, **starvation**의 문제가
 존재
	- 우선순위가 낮은 프로세스는 지속적으로 추가되는
	 우선순위가 높은 프로세스에 밀려 오랜 기간동안 대기할
	 수 있음
	- **aging**을 통해 오래된 프로세스의 우선순위를
	 높이는 방식으로 해결하기도 함
- 전체적으로는 우선순위 스케쥴링을 사용하면서 동일한
 우선순위의 프로세스들만 round\-robin 방식으로 스케쥴링하는
 혼합 방식을 사용하기도 함


## 5\.3\.5 Multilevel Queue Scheduling


- 우선순위 스케쥴링, round\-robin 스케쥴링 모두에서 단일 큐에
 프로세스가 들어가게 되면 실행할 프로세스를 찾기 위해
 $O(n)$의 탐색 시간이 걸릴 수도 있음
- 실제로는 각 고유한 우선순위마다 큐를 따로 두고 가장 높은
 우선순위의 큐의 프로세스를 스케쥴하기도 함
- **Multilevel queue**는 우선순위 스케쥴링이 라운드
 로빈과 결합될 때 유리
- foreground 프로세스와 background 프로세스 큐로 나누기도 함
	- 둘은 response time 요구사항이 다르므로 다른 스케쥴링이
	 필요할 수 있음
- 할당받은 큐가 바뀌지는 않음


## 5\.3\.6 Multilevel Feedback Queue Scheduling


- 프로세스가 큐에서 큐로 이동할 수 있음
- CPU burst가 높은 프로세스를 낮은 우선순위의 큐로 옮겨서
 I/O bound 프로세스가 높은 우선순위로 실행되도록 할 수 있음
	- time quantum 채운 프로세스는 CPU burst 프로세스로
	 판단하여 하위 큐로 이동
