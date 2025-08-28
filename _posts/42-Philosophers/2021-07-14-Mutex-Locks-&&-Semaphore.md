---
layout: post
title: "Mutex Locks && Semaphore"
date: "2021-07-14 23:22:15 +0900"
categories:
  - 42-Philosophers
---
## Mutex lock


- mutex : **mut**ual **ex**clusion
 (상호 배제)
- 임계 영역 진입할 때 열쇠(lock) 가지고 들어가고, 나올 때는
 열쇠 반납하는 느낌
 
```False
while (true) {
acquire lock
   critical section
release lock
   remainder section
}
```
- `acquire()`와 `release()`의 두 함수
- `available`의 불리안 변수. : lock을 얻을 수
 있는 상황이면 true, 아니면 false.



```False
acquire() {
    while (!available)
        ; /* busy wait */
    available = false;
}
release() {
    available = true;
}
```

- `acquire()`와 `release()`과정도
 atomically하게 이루어져야 함. 그렇지 않으면 상호 간섭 발생
 가능성 존재. (커널 설계 단계에서 이미 고려하였으니
 여기서는 고려하지 말자)



 위와 같이 구현하는 경우 **Busy waiting**문제가
 생긴다.
 


- `acquire()`에서 루프를 계속 도는 문제
- 단일 CPU 코어에서는 여러 프로세스가 코어를 나눠서 쓰므로
 더더욱 문제
- 프로세스 나눠서 실행하는 경우에 아무 의미 없이 루프 도는데
 CPU사이클을 소모하게 되는 것이다.


### Spinlock


- busy waiting방법을 사용하는 뮤텍스 락의 종류를
 Spinlock이라 부른다.
- lock이 available해질 때까지 프로세스가 계속 spin한다.
- 단점만 있는 것은 아니고 장점도 있다.
	- *코어가 여러 개*인 경우 스핀을 돌다가 lock이
	 available해지면 바로 임계 영역으로 진입 가능.
	- 만약 spinlock이 아닌 경우 wait queue에서 있다가 ready
	 queue를 거쳐 오는 등 context switch에 시간이 소모됨.


다음의 예제 코드를 통해 확인.



```False
#include <stdio.h>
#include <pthread.h>

int sum = 0;

pthread_mutex_t mutex;

void *counter(void *param)
{
    int k;
    for (k = 0; k < 10000; k++) {
        /* entry section */
        pthread_mutex_lock(&mutex);

        /* critical section */
        sum++;

        /* exit section */
        pthread_mutex_unlock(&mutex);

        /* remainder section */
    }
    pthread_exit(0);
}

int main()
{
    pthread_t tid1, tid2;
    pthread_mutex_init(&mutex, NULL);
    pthread_create(&tid1, NULL, counter, NULL);
    pthread_create(&tid2, NULL, counter, NULL);
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    printf("sum = %d\n", sum);
}
```



---


## Semaphore


- 신호장치, 신호기의 의미
- Semaphore `S`는


	- 정수형 변수이고,
	- `wait()`, `signal()`의 두 atomic
	 operation을 통해서만 접근 가능. (`P()`,
	 `V()`로 부르기도 함)  
	다음의 예시를
	 보자.



```False
wait(S) {
    while (S <= 0)
        ; // busy wait
    S--;
}

signal(S) {
    S++;
}
```


 mutex가 여러 개 있는 느낌 (binary semaphore ≒ mutex lock)
 


- 세마포어를 사용해서 동기화 문제를 해결하는 예시


	- 프로세스 `P1`과 `P2`가
	 concurrent하게 돈다고 가정.
	- `P1`은 `S1`을 실행.
	 `P2`는 `S2`를 실행.
	- `S2`는 `S1`이 실행된 후
	 실행되어야 한다고 가정.
	- `P1`과 `P2`가 세마포어를
	 공유하도록 하고 0으로 초기화
```False
S1;
signal(synch);
```


```False
wait(synch);
S2;
```


`P2`는 세마포어가 0이므로 wait에서 대기.
 `P1`이 `S1`을 실행한 후 signal에서
 세마포어를 증가시켜주게 되면 비로소 `S2`실행
 가능.
- 세마포어의 구현



```False
typedef struct {
int value;
struct process *list;
} semaphore;

```



 wait(semaphore \*S) {  

 S\-\>value\-\-;  

 if (S\-\>value \< 0\) {  

 add this process to S\-\>list;  

 sleep();  

 }  
}
 



 signal(semaphore \*S) {  

 S\-\>value\+\+;  

 if (S\-\>value \<\= 0\) {  

 remove a process P from S\-\>list;  

 wakeup(P);  

 }  
}
 



```False
이렇게 프로세스 스스로 suspending을 하도록 하여 busy waiting을 방지할 수 있다.

-    다음은 실습 코드
```cpp
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>

int sum = 0;  // a shared variable

sem_t sem;

void *counter(void *param)
{
    int k;
    for (k = 0; k < 10000; k++) {
        /* entry section */
        sem_wait(&sem);

        /* critical section */
        sum++;

        /* exit section */
        sem_post(&sem);

        /* remainder section */
    }
    pthread_exit(0);
}

int main()
{
    pthread_t tid1, tid2;
    sem_init(&sem, 0, 1);
    pthread_create(&tid1, NULL, counter, NULL);
    pthread_create(&tid2, NULL, counter, NULL);
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    printf("sum = %d\n", sum);
}
```


 (OS X에서는 sem\_init대신 sem\_open을 써야하는 것 같다. 확인
 필요)
