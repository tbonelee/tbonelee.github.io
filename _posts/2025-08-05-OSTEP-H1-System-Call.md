---
layout: post
title: "[OSTEP-Xv6-Lab] H1. 시스템 콜 추가"
date: "2025-08-05 18:00 +0900"
tags: [operating-system, xv6, system-call, risc-v, kernel, ostep]
---

[Operating Systems: Three Easy Pieces](https://pages.cs.wisc.edu/~remzi/OSTEP/)는 마지막에 실습 과제를 포함하고 있다. 예전에 책을 보기는 했지만 실습을 스킵했더니 놓치는 게 많았던 거 같아서 하나씩 진행해보려고 한다.

과제는 MIT에서 교육용으로 만든 RISC-V OS [Xv6](https://github.com/mit-pdos/xv6-riscv)를 기반으로 진행된다.

# Task

> Your new system call should look have the following return codes and parameters:
```c
int getreadcount(void)
```
> Your system call returns the value of a counter (perhaps called readcount or something like that) which is incremented every time any process calls the read() system call. That's it!

이번 과제는 `read()` 시스템콜의 호출 횟수를 반환하는 새로운 시스템콜 `getreadcount()`를 추가하는 것이다. 간단한 과제이지만 시스템콜이 커널에 추가되는 전체 과정을 이해할 수 있는 좋은 기회다.

# 시스템콜 동작 원리

코드를 작업하기 전에 시스템콜이 어떻게 동작하는지 먼저 살펴보자.

## 유저의 시스템 콜 호출 과정

우선 유저 프로그램 개발자는 [`user/user.h`](https://github.com/tbonelee/xv6-riscv/blob/refs/tags/h1/user/user.h)에 선언된 시스템콜 함수를 호출할 수 있다.

해당 프로그램을 빌드하게 되면 실제로 호출되는 코드의 내용은 `user/usys.S`에서 확인할 수 있다.

Xv6에서는 빌드 과정에서 [`user/usys.pl`](https://github.com/tbonelee/xv6-riscv/blob/refs/tags/h1/user/usys.pl)를 통해 동적으로 생성된다.

예를 들어 `read()`함수의 어셈블리 스텁은 다음과 같다.
```assembly
.global read
read:
 li a7, SYS_read # "kernel/syscall.h"에 정의된 매크로로 몇 번 시스템 콜인지 나타내는 정수
 ecall           # 시스템 콜 트랩 발생 (유저 모드 -> 커널 모드)
 ret             # 호출했던 C 코드로 복귀
```

여기서 유저 프로그램은 `a7` 레지스터에 시스템 콜 번호를 저장하고, `ecall` 인스트럭션을 실행한다.

유저 프로그램이 `ecall`을 실행하면 CPU는 커널 모드로 전환하고 `stvec` 레지스터에 저장된 주소 [`uservec`](https://github.com/tbonelee/xv6-riscv/blob/3bbc3d42b1086f049388fd11dde54b56972d481b/kernel/trampoline.S#L22)으로 점프한다.

(유저 프로그램을 초기화할 때 `stvec` 레지스터에 미리 `uservec` 코드 주소를 등록해놓게 된다.)

`uservec`에서는 유저 프로그램의 레지스터를 트랩프레임에 저장하고 커널 스택 포인터와 커널 페이지 테이블 주소를 레지스터에 등록한다.

(커널 코드를 실행하기 위한 준비)

그 후 [`kernel/trap.c`](https://github.com/tbonelee/xv6-riscv/blob/3bbc3d42b1086f049388fd11dde54b56972d481b/kernel/trap.c#L37)의 `usertrap()`으로 넘어가서 실질적인 트랩 핸들러 코드를 실행하게 된다.

`usertrap()`은 여러 시스템콜을 처리하게 되는데 `scause` 레지스터가 8인 것을 통해 시스템 콜 호출인 것을 파악하고, `syscall()`함수를 호출한다.

```c
if (r_scause() == 8) {
    ...
    syscall();
}
```

[`syscall()`](https://github.com/tbonelee/xv6-riscv/blob/3bbc3d42b1086f049388fd11dde54b56972d481b/kernel/syscall.c#L134)이 하는 역할은 단순히 유저 프로그램이 `a7`에 저장한 시스템 콜 번호를 확인해서 적절한 시스템 콜 코드를 실행하고 결과를 `a0`레지스터에 담아주는 것이다.

```c
syscall(void)
{
  int num;
  struct proc *p = myproc();

  num = p->trapframe->a7;
  if(num > 0 && num < NELEM(syscalls) && syscalls[num]) {
    // Use num to lookup the system call function for num, call it,
    // and store its return value in p->trapframe->a0
    p->trapframe->a0 = syscalls[num]();
  } else {
    printf("%d %s: unknown sys call %d\n",
            p->pid, p->name, num);
    p->trapframe->a0 = -1;
  }
}
```

각 시스템콜 함수의 인자는 유저 프로그램이 `a1` ~ `a5` 레지스터에 넣어두었던 것을 프로세스 트랩프레임을 통해 전달받아 사용하게 된다.

시스템콜 함수가 모든 작업을 다 마치면 다시 `usertrap()`의 마지막에서 `kernel/trampoline.S`의 `userret`을 실행하고, 여기서 다시 유저 프로세스의 레지스터를 복원하게 된다.

# 구현

`getreadcount()` 시스템콜을 추가하기 위해서는 다음과 같은 파일들을 수정해야 한다:

-  `user/user.h`에 유저가 사용할 수 있는 함수 정의를 작성해줘야 한다.

```diff
+ uint64 getreadcount(void);
```

-  `user/usys.S`에 `getreadcount` 어셈블리 스텁이 생성되도록 `user/usys.pl`을 수정해줘야 한다.

```diff
entry("read");
+ entry("getreadcount");
```

- `kernel/syscall.h`에 새 시스템콜 번호를 정의해줘야 한다.

```diff
#define SYS_close  21
+ #define SYS_getreadcount 22
```

- `kernel/syscall.c`의 `syscalls` 배열에 함수 포인터를 등록해줘야 한다.

```diff
[SYS_close]   sys_close,
+ [SYS_getreadcount] sys_getreadcount,
```

- `kernel/sysfile.c`에 `syscall()`이 호출할 실제 `getreadcount`의 로직을 담고 있는 `sys_getreadcount()` 추가

```c
static _Atomic uint64 read_count = 0; // Global read count for sys_getreadcount
```

멀티 코어 환경에서 레이스 컨디션을 방지하기 위해 `_Atomic` 변수로 선언하고 `sys_read()`의 호출마다 증가시켜준다.

그리고 `sys_getreadcount()`에서는 그냥 이걸 반환해주면 된다.

```c
uint64
sys_getreadcount(void)
{
  return read_count;
}
```

## 리눅스 커널 코드에서 관련된 코드 찾아보기 (RISC-V)

- 트랩 핸들러 주소 등록 [`arch/riscv/kernel/entry.S`](https://github.com/torvalds/linux/blob/8d084337a32fde0ffa59d5f70d07a54987911ba1/arch/riscv/kernel/entry.S#L453)
- 시스템콜을 처리하는 트랩 핸들러 코드 [`arch/riscv/kernel/traps.c`](https://github.com/torvalds/linux/blob/8d084337a32fde0ffa59d5f70d07a54987911ba1/arch/riscv/kernel/traps.c#L327)
- 시스템콜 테이블 관련 코드 [`arch/riscv/kernel/syscall_table.c`](https://github.com/torvalds/linux/blob/master/arch/riscv/kernel/syscall_table.c)
