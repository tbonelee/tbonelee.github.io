---
layout: post
title: "[계산이론] Nondeterministic Finite Accepter"
date: "2023-11-05 23:27:56 +0900"
categories:
  - 계산이론,오토마타
---
## Nondeterministic Finite Accepter




---


def)



 A **nondeterministic finite accepter** or
 **nfa** is defined by the quintuple
 


$$<br/>M = (Q, \Sigma, \delta, q_{0}, F) ,<br/>$$



 where $Q$, $\Sigma$, $q_{0}$, $F$ are defined as for
 deterministic finite accepters, but  
$$<br/>\delta : Q
                    \times ( \Sigma \cup { \lambda } ) \rightarrow 2^{Q}.<br/>$$
 




---


### DFA와의 차이


- $\delta$의 range(치역)가 $2^{Q}$의 powerset.
	- 여러 state로 이동 가능
	- 공집합도 가능
- input으로 $\lambda$ 허용
	- symbol을 읽지 않는 transition 가능


### NFA의 승인 방식



 input string을 모두 처리한 다음 머신이 승인 상태에 놓이는
 이동 경로가 하나 이상 존재한다면 accept되고 그렇지 않다면
 reject된다.
 


### Extended transition function.



 확장 전이 함수 $\delta^{\ast}$는 다음을 만족해야 한다.  
If
 $\delta^{\ast}(q_{i},w) = Q_{j}$, then $Q_{j}$ is the set of
 all possible states the automation may be in, having started
 in state $q_{i}$ and having read $w$.
 




---



 def)  
For an nfa, the extended transition function is
 defined so that $\delta^{\ast}(q_{i}, w)$ contains $q_{j}$
 if and only if there is a walk in the transition graph from
 $q_{i}$ to $q_{j}$ labeled $w$. This holds for all $q_{i},
                    q_{j} \in Q$, and $w \in \Sigma^{\ast}$.
 




---


nfa로 accept되는 language는 다음과 같이 정의한다.




---



 def)  
The language $L$ accepted by an nfa $M = (Q,
                    \Sigma, \delta, q_{0}, F)$ is defined as the set of all
 strings accepted in the above sense. Formally,  
$$<br/>L(M)
                    = { w \in \Sigma ^{\ast} : \delta^{\ast} (q_{0}, w) \cap F
                    \neq \emptyset }.<br/>$$  
In words, the language
 consists of all strings $w$ for which there is a walk
 labeled $w$ from the initial vertex of the transition graph
 to some final vertex.
 




---
