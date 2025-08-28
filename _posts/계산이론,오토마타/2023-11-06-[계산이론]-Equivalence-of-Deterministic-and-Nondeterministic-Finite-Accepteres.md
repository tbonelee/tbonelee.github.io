---
layout: post
title: "[계산이론] Equivalence of Deterministic and Nondeterministic Finite Accepteres"
date: "2023-11-06 00:27:59 +0900"
categories:
  - 계산이론,오토마타
---
finite accepter의 equivalence는 두 accepter가 accept하는
 set이 동일하고, reject하는 set이 동일하다고 정의.
 




---


def)



 Two finite accepters, $M_{1}$ and $M_{2}$, are said to be
 equivalent if
 


$$<br/>L(M_{1}) = L(M_{2}),<br/>$$


that is, if they both accept the same language.




---




---


Theorem)



 Let $L$ be the language accepted by a nondeterministic
 finite accepter $M_{N} = (Q_{N}, \Sigma, \delta_{N}, q_{0},
                    F_{N} )$. Then there exists a deterministic finite accepter
 $M_{D} = (Q_{D}, \Sigma, \delta_{D}, { q_{0} }, F_{D})$ such
 that
 


$$<br/>L = L(M_{D})<br/>$$



**Proof:** Given $M_{N}$, we use the procedure
 *nfa\-to\-dfa* below to construct the transition graph
 $G_{D}$ for $M_{D}$. To understand the construction,
 remember that $G_{D}$ has to have certain properties. Every
 vertex must have exactly $|\Sigma|$ outgoing edges, each
 labeled with a different element of $\Sigma$. During the
 construction, some of the edges may be missing, but the
 procedure continues until they are all there.
 


**procedure: nfa\-to\-dfa**


1. Create a graph $G_{D}$ with vertex ${q_{0} }$. Identify
 this vertex as the initial vertex.
2. Repeat the following steps until no more edges are
 missing.  
Take any vertex ${ q_{i} , q_{j} , \cdots ,
                      q_{k} }$ of $G_{D}$ that has no outgoing edge for some $a
                      \in \Sigma$. Compute $ \delta_{N}^{ \ast } ( q_{i} , a)$,
 $\delta_{N}^{\ast} (q_{j}, a)$, ... $\delta_{N}^{\ast}
                      (q_{k}, a)$. If  
$$<br/>\delta_{N}^{\ast} (q_{i}, a)
                      \cup \delta_{N}^{\ast}(q_{j}, a) \cup \cdots \cup
                      \delta_{N}^{\ast}(q_{k}, a) = { q_{l}, q_{m}, \cdots,
                      q_{n} },<br/>$$  
create a vertex for $G_{D}$ labeled
 ${q_{l} , q_{m} , \cdots, q_{n} }$ if it does not already
 exist.  
Add to $G_{D}$ an edge from ${ q_{i}, q_{j},
                      \cdots, q_{k} }$ to ${ q_{l}, q_{m}, \cdots, q_{n} }$ and
 label it with $a$.
3. Every state of $G_{D}$ whose label contains any $q_{f} \in
                      F_{N}$ is identified as a final vertex.
4. If $M_{N}$ accepts $\lambda$, the vertex ${ q_{0} }$ in
 $G_{D}$ is also made a final vertex.


(생략)




---



 결국 NFA의 상태들의 집합을 DFA의 하나의 상태로 대응시키는
 과정.
