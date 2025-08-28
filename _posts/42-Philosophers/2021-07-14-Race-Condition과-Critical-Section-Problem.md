---
layout: post
title: "Race Condition과 Critical Section Problem"
date: "2021-07-14 18:25:50 +0900"
categories:
  - 42-Philosophers
---
cf) 내용에 있는 오류로 발생하는 문제는 책임지지 않습니다.
 


## Data race


다음 코드 예시를 보자



```False
#include <stdio.h>
#include <pthread.h>

int sum;

void *run(void *param)
{
    int i;
    for (i = 0; i < 10000; i++)
        sum++;
    pthread_exit(0);
}

int main()
{
    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, run, NULL);
    pthread_create(&tid2, NULL, run, NULL);
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    printf("%d\n", sum);
}
```


 메인 함수에서 `tid1`과 `tid2`라는
 스레드를 생성하였고 각각의 스레드가 `run`이라는
 함수를 실행하도록 했다.  
pthread\_join함수를 통해
 `main`스레드는 `tid1`과
 `tid2`가 종료할 때까지 기다리도록 하였다.
 



 코드만 보았을 때는 마지막에 `printf`에서 20000이
 출력되어야 할 것 같지만 20000이 출력되기도 하고 다른 값이
 출력되기도 한다.
 



 왜 그렇게 되는 걸까?  
어셈블리어 수준으로 분해해보면
 조금 간단하게 이해할 수 있다.
 



 우선 tid1의 스레드에서 메모리에 있는 값을 변경시키기
 위해서는 다음 세 단계의 인스트럭션이 필요하다.
 


- register1 \= sum
- register1 \= register1 \+ 1
- sum \= register1  
마찬가지로 tid2의 스레드에서도 다음
 세 단계의 인스트럭션이 필요하다.
- register2 \= sum
- register2 \= register2 \+ 1
- sum \= register2  
두 스레드를 concurrent하게 실행시키기
 위해서 중간중간에 스레드 간 컨텍스트 스위칭(context
 switching)이 발생한다.  
만약 `sum == 0`인
 상황에서 스레드 1을 실행하고 있다고 가정해 보자.  
이때
 `register1`에 `sum == 0`을 저장하고
 `register1 = register1 + 1`까지 수행하고 스레드
 2로 컨텍스트 스위칭이 발생하면 `register1`에는
 1의 값이 저장되어 있을 것이다.  
그 상태에서 스레드 2가
 계속 실행되면 sum의 값이 증가하게 된다.  
하지만 나중에
 다시 스레드1로 컨텍스트 스위칭이 발생하게 되고
 스레드1에서는 `sum = register1(== 1)`을
 실행하게 되어 `sum = 1`로 스레드2에서 실행한
 결과가 무용지물이 된다.



 위의 예시는 하나의 예시일 뿐이고 다양한 경우가 존재할 수
 있다.  
이러한 방식으로 여러 스레드가 공유자원에 동시에
 접근할 때 발생하는 데이터에 대한 경쟁 상황을
 **데이터 레이스(data race)**라고 부른다.  
cf)
 하드웨어 스레드 vs. 소프트웨어 스레드 :
 <https://juneyr.dev/thread>





---


## 임계 영역 문제(The Critical Section Problem)


프로세스간 데이터가 공유되는 \_임계 영역\_이 존재.



 한 프로세스가 임계 영역에서 명령을 수행할 때 다른 프로세스가
 접근하지 못하도록 해야 하는 문제가 임계 영역 문제.
 



 임계 영역 문제는 프로세스 간 활동을 동기화하여 데이터 공유가
 원활하게 이루어지는 프로토콜을 수립하는 문제.
 


임계 영역 문제 해결은 다음 세 가지를 만족해야 한다.


1. 상호 배제(Mutual Exclusion) : 프로세스 $P_i$가 임계
 영역에서 실행 중이면 다른 프로세스가 임계 영역에서 실행
 중이어서는 안 된다.
2. Progress (deadlock을 회피) : 임계 영역에 아무 프로세스도
 없는데 임계 영역에서 실행하고자 하는 프로세스가 진입
 못하는 상황이 있어서는 안 된다.
3. Bounded Waiting (starvation을 회피) : 프로세스가 임계
 영역에 한 번 진입하고 나서 다시 필요에 의해 진입할 수 있는
 시간에도 제한이 있어야 한다(?). 다른 프로세스가 진입
 못해서 '아사'하는 상태가 발생하지 않게 하기 위해.
 즉, 몇 놈만 계속 들어가고 나머지는 못 들어가는 상황이
 있어서는 안 된다는 의미인듯.



 \=\> 다 푸는 것은 어려워서 현실에서는 다 풀지 않는 경우가
 많다.
 



 싱글\-코어 환경에서는 공유 자원 사용할 때 다른 프로세스의
 인터럽트를 막으면 된다.
 



 하지만 멀티\-코어 환경이라면? \-\> 하나 막아도 다른 코어에서
 실행할 수 있다. 그렇다고 이걸 막으면 프로세서 효율이 너무
 저하되는 문제.
 




---


## 피터슨 솔루션



 '거의' 완전한 해결책 but, 하드웨어 수준에서 완벽하지
 않음.
 



 \=\> 경쟁 상태를 해결하기 위해 분해할 수 없는 물리적인
 작업(Atomic operation)을 만듦. 자바에서는 이를 사용 가능
 




---



 하드웨어 수준이 아닌 조금 더 고급 단계에서 해결하기 위해
 뮤텍스(Mutex)와 세마포어(Semaphore)가 등장..
