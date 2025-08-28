---
layout: post
title: "[계산이론] Deterministic Finite Accepter"
date: "2023-11-05 20:24:12 +0900"
categories:
  - 계산이론,오토마타
---
오토마타의 한 종류인 finite accepter의 특징은 다음과 같다.
 


- 임시 storage를 갖지 않음
- input 파일의 내용을 수정할 수 없기 때문에 계산 과정에서
 무언가를 '기억'하는 것에는 한계를 가짐.
- 제한된 정보는 control unit의 state를 조정하는 것을
 통해서만 기억될 수 있다.
- 그런데 그러한 state 수는 유한하므로 finite 오토마타는 특정
 시점에 저장되는 정보가 엄격하게 제한되는 상황만 처리
 가능하다.


## Deterministic Finite Accepters




---



 def)  
A **deterministic finite accepter** or
 **dfa** is defined by the quintuple  
$$<br/>M = (Q,
                    \Sigma, \delta, q_{0}, F),<br/>$$  
where  
$Q$ is a
 finite set of **internal states**,  
$\Sigma$ is a
 finite set of symbols called the
 **input alphabet**,  
$\delta : Q \times \Sigma
                    \rightarrow Q$ is a total function called the
 **transition function**,  
$q_{0} \in Q$ is the
 **initial state**,  
$F \subseteq Q$ is a set of
 **final states**.
 




---


### 동작 방식



 initial 시점에 오토마타는 state $q_{0}$에 있다고 가정.  
매
 move마다 input 메커니즘은 한 포지션씩 이동하고 하나의 input
 symbol을 읽어낸다.  
string의 끝에 도달했을 때 final
 state중 하나의 state에 있으면 string을 accept하고 아니라면
 reject한다.
 



 한 내부 state에서 input symbol을 입력 받았을 때의
 transition은 $\delta$에 의해 정의되고 다음과 같이 표기할 수
 있다.  
$$<br/>\delta(q_{0}, a) = q_{1}<br/>$$  
($q_{0}$에서
 $a$를 입력 받으면 $q_{1}$으로 이동함을 의미)
 


### Transition Graph로 표현



 finite automata의 직관적 이해를 위해
 **transition graphs**를 사용할 수 있다.
 



 각 vertex 각각의 state를 의미하고, 각 edge는 transition을
 의미한다.
 



 vertex label은 state의 이름을 의미하고 edge의 label은 input
 symbol의 현재 값을 의미한다.
 



 Initial state는 label이 붙지 않고 외부에서 진입하는 화살표로
 가리켜진다.
 



 Final state는 double circle로 그려진다.
 



 만약 $M = (Q, \Sigma, \delta, q_{0}, F)$가 dfa라면 연관된
 transition graph $G_{M}$은 정확히 $|Q|$개의 vertex를 가지고
 각각 서로 다른 $q_{i} \in Q$로 label 붙여진다.  
$q_{0}$와
 관련된 vertex는 **initial vertex**로 불리고, $q_{f} \in
                    F$로 label 붙여진 vertex는 **final vertices**로 불린다.
 



 dfa $$(Q, \Sigma, \delta, \q_{0}, F)$의 정의에서 transition
                    graph를 구성하는 것과 그 역 모두 trivial한 과정이다.
                  </p>
<h3 data-ke-size="size23">Extended transition function</h3>
<p data-ke-size="size16">
                    Extended transition function $\delta^{\ast} : Q \times
                    \Sigma ^{\ast} \rightarrow Q$의 두번째 인자는 단일 symbol
                    대신 string이고 결과값은 string의 symbol을 차례대로 전부
                    읽고 난 후의 state이다.
                  </p>
<p data-ke-size="size16">
                    extended transition function을 사용하는 것이 설명의 편의성을
                    가져다 줄 수 있다.
                  </p>
<h2 data-ke-size="size26">Language와 DFA</h2>
<hr data-ke-style="style1"/>
<p data-ke-size="size16">
                    def)<br/>The language accepted by a dfa $M = (Q, \Sigma,
                    \delta, q_{0}, F)$ is the set of all strings on $\Sigma$
                    accepted by $M$. In formal notation,<br/>$$  
L(M) \= { w
 \\in \\Sigma^{\\ast} : \\delta^{\\ast} (q\_{0}, w) \\in F }  
$$
                  </p>
<hr data-ke-style="style1"/>
<p data-ke-size="size16">
                    여기서 $\delta$와 $\delta ^{\ast}$는 모두 total function이기
                    때문에 모든 (state, input)에 대해(total) 단 하나의
                    transition만(function) 정의되어야 한다.
                  </p>
<p data-ke-size="size16">
                    이러한 특성 때문에 deterministic하다고 부르는 것이다.
                  </p>
<p data-ke-size="size16">
                    dfa는 $\Sigma^{\ast}$의 모든 string을 처리하여 accept or
                    reject 한다. 따라서 다음이 성립.
                  </p>
<p data-ke-size="size16">
                    $$  
\\overline{L(M)} \= { w \\in \\Sigma ^{\\ast} :
 \\delta^{\\ast} (q\_{0}, w) \\in F }  
$$
                  </p>
<p data-ke-size="size16">
<b>trap state</b> : 한 번 들어온 이상 다른 상태로 벗어날 수
                    없는 state
                  </p>
<h2 data-ke-size="size26">Regular Language</h2>
<p data-ke-size="size16">
                    모든 finite 오토마타는 몇몇 language를 accept한다.
                  </p>
<p data-ke-size="size16">
                    그렇게 승인될 수 있는 모든 가능한 language는 하나의 set을
                    이룬다.
                  </p>
<p data-ke-size="size16">
                    앞으로 종종 언어를 set으로 구분하게 되는데 특별히 언어의
                    set을 <b>family</b>라고 부른다.
                  </p>
<hr data-ke-style="style1"/>
<p data-ke-size="size16">
                    def)<br/>A language $L$ is called <b>regular</b> if and
                    only if there exists some deterministic finite accepter $M$
                    such that<br/>$$  
L \= L(M)  
$$
 




---
