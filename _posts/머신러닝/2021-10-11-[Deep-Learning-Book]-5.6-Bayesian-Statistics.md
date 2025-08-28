---
layout: post
title: "[Deep Learning Book] 5.6 Bayesian Statistics"
date: "2021-10-11 16:54:40 +0900"
categories:
  - 머신러닝
---
- frequentist
	- 단일한 $\theta$의 값을 추정하고 이 하나의 추정을
	 바탕으로 모든 예측 수행
	- $\theta$의 참값은 고정되어 있지만 알려져 있지 않음
	- 점추정치 $\hat{\theta}$는 랜덤하게 관측되는 데이터셋의
	 함수이므로 확률 변수다.
- Bayesian
	- 확률을 어떤 지식의 상태에 대한 확실한
	 정도(certainty)를 나타내기 위해 사용한다.
	- $\theta$의 모든 가능한 값들을 고려하여 예측을
	 수행한다.
	- 데이터셋은 직접적으로 관측된 것이므로 non\-random이다.
	 (관찰되기 전에도 non\-random이라는 뜻이 아님)
	- true parameter $\theta$는 알려져 있지 않을뿐더러
	 unceratain하므로 확률 변수로 취급된다.




---


- **사전적 확률 분포(prior probability distribution,
 $p(\theta)$):**
	- 데이터 관찰 이전의 우리가 가지고 있는 $\theta$에 대한
	 지식
	- prior라고만 부르기도 함
	- ex) 우리는 동전을 던졌을 때 각 면이 나올 확률은 0\.5가
	 될 것이라는 사전적 지식을 가지고 있음



 사전적 확률 분포에 대한 각자의 신념(belief)이 있는 상황에서
 data 표본 집합 ${x^{(1)} ,..., x^{(m)}}$을 얻게 되었다고
 하자.
 



 그러면 이 dataset을 가지고 우리가 가지고 있는 확률 분포에
 대한 신념을 다음과 같이 수정할 수 있다.
 



 $$p(\theta | x^{(1)} , ... , x^{(m)} ) = \frac{p(x^{(1)} ,
                    ... , x^{(m)} | \theta) p(\theta) }{p(x^{(1)}, ... , x^{(m)}
                    )}$$
 



 베이지안 추정을 할 때 보통 prior는 유니폼 분포나 가우시안
 분포 같이 엔트로피가 높은 것으로 정하고, 데이터 관찰을 통해
 posterior가 엔트로피를 잃고 가능성 높은 파라미터의 근처에
 집중하여 분포할 수 있도록 갱신해나간다.
 




---



 MLE 추정과 두 가지 주요한 차이점이 있다.
 


1. MLE는 $\theta$에 대한 점추정값을 통해 예측하지만 베이지안
 접근은 $\theta$에 대한 전체 분포를 사용하여 예측을
 수행한다.
	- $m$개의 example을 관찰한 후 $x^{(m+1)}$의 분포를
	 예측할 때 다음의 식을 사용한다.
	- $p(x^{(m+1)} | x^{(1)},... , x^{(2)} ) = \int
                          p(x^{(m+1)} | \theta) p(\theta| x^{(1)} , ... ,
                          x^{(m)} ) d\theta$
		- cf) $x^{(m+1)}$의 확률을 posterior를 가지고 가중
		 평균한 의미도 될 수 있음
	- $m$개의 데이터셋을 관찰한 후에도 $\theta$에 대해
	 불확실성을 가지고 있다면 예측에 이러한 불확실성도
	 반영할 수 있게 된다.
	- 베이지안 통계가 확률 법칙을 통해 불확실성을 반영하는
	 것과는 다르게 frequentist는 $\theta$ 점추정치의 분산을
	 사용한다. (다소 ad hoc하다)
2. 베이지안 추정에는 prior 분포가 개입된다는 점이 큰 차이
	- prior의 영향으로 확률 분포가 priori가 선호하는
	 parameter space에 가까워지게 됨
	- 베이지안을 비판하는 쪽에서는 주관적인 판단이 예측에
	 들어간다고 비판



 베이지안 추정은 training 데이터가 적을 때 더
 generalization이 잘 되는 측면이 있지만, training 데이터가
 많을 때는 계산 cost가 크다는 단점이 존재
 




---


## 베이지안 선형 회귀



 $\boldsymbol{x} \in \mathbb{R}^{n}$을 선형 맵핑하여 $y \in
                    \mathbb{R}$을 예측하고자 함
 



 이 때 예측의 파라미터는 벡터 $\boldsymbol{\omega} \in
                    \mathbb{R}^{n}$으로 다음의 식과 같다
 



 $$\hat{y} = \boldsymbol{\omega}^{\mathsf{T}}
                    \boldsymbol{x}$$
 



 $m$개의 training sample인 $(\boldsymbol{X}^{(train)} ,
                    \boldsymbol{y}^{(train)})$을 가지고 한 모든 $y$에 대한
 예측을 하나의 식으로 나타내면 다음과 같다.
 



 $$\hat{\boldsymbol{y}} ^{(train)} = \boldsymbol{X}
                    ^{(train)} \boldsymbol{\omega}$$
 



 $\boldsymbol{y}^{(train)}$가 조건부 가우시안 분포를 따른고,
 $\boldsymbol{y}^{(train)}$의 covariance 행렬이 단위행렬이라
 가정하고 일반적인 MSE식에 따라 다음과 같이 쓸 수 있다.
 



 $$\begin{aligned}<br/>p\left({\boldsymbol{y}}^{(train)} |
                    {\boldsymbol{X}}^{(train)} , \boldsymbol{\omega} \right)
                    &amp; = \mathcal{N} \left({\boldsymbol{y}}^{(train)} ;
                    {\boldsymbol{X}}^{(train)} \boldsymbol{\omega} ,
                    \boldsymbol{I} \right) \\ &amp; \propto \text{exp} \left(-
                    \frac{1}{2} {\left( {\boldsymbol{y}}^{(train)} -
                    {\boldsymbol{X}}^{(train)} \boldsymbol{\omega}
                    \right)}^{\mathsf{T}} \left({\boldsymbol{y}}^{(train)}-
                    {\boldsymbol{X}}^{(train)} \boldsymbol{\omega}\right)
                    \right) \\ \end{aligned}$$
 



 모형의 파라미터 벡터인 $\boldsymbol{\omega}$의 posterior
 분포를 구하기 위해서는 우선 prior 분포가 무엇인지 정해야
 한다.
 



 실수 파라미터에 대해 prior 분포로 다음과 같이 가우시안
 분포를 사용하는 경우가 흔하다.
 



 $$p\left(\boldsymbol{\omega}\right) = \mathcal{N}
                    \left(\boldsymbol{\omega} ; \boldsymbol{\mu}_{0},
                    \boldsymbol{\Lambda}_{0} \right) \propto \exp \left( -
                    \frac{1}{2} \left( \boldsymbol{\omega} -
                    \boldsymbol{\mu}_{0}\right) ^{\mathsf{T}}
                    \boldsymbol{\Lambda}_{0}^{-1} \left( \boldsymbol{\omega} -
                    \boldsymbol{\mu}_{0}\right) \right)$$
 



 여기서 $\boldsymbol{\mu}_{0}$와 $\boldsymbol{\Lambda}_{0}$는
 각각 prior 분포의 mean 벡터와 covariance 행렬이다.
 



 이제 필요한 것들을 명확히 했으니 **posterior** 분포를
 다음과 같이 작성할 수 있다.
 



 $$\begin{aligned} p\left(\boldsymbol{\omega} |
                    \boldsymbol{X}, \boldsymbol{y} \right) &amp; \propto
                    p\left(\boldsymbol{y} | \boldsymbol{X} , \boldsymbol{\omega}
                    \right) p \left(\boldsymbol{\omega} \right) \\ &amp; \propto
                    \exp \left( - \frac{1}{2} \left( \boldsymbol{y} -
                    \boldsymbol{X} \boldsymbol{\omega} \right)^{\mathsf{T}}
                    \left( \boldsymbol{y} - \boldsymbol{X} \boldsymbol{\omega}
                    \right) \right) \exp\left(- \frac{1}{2}
                    \left(\boldsymbol{\omega} -
                    \boldsymbol{\mu}_{0}\right)^{\mathsf{T}}
                    \boldsymbol{\Lambda}_{0}^{-1} \left( \boldsymbol{\omega} -
                    \boldsymbol{\mu}_{0}\right)\right) \\ &amp; \propto \exp
                    \left( -\frac{1}{2}\left( -2\boldsymbol{y}^{\mathsf{T}}
                    \boldsymbol{X}\boldsymbol{\omega} +
                    \boldsymbol{\omega}^{\mathsf{T}}\boldsymbol{X}^{\mathsf{T}}
                    \boldsymbol{X} \boldsymbol{\omega} +
                    \boldsymbol{\omega}^{\mathsf{T}}
                    \boldsymbol{\Lambda}_{0}^{-1} \boldsymbol{\omega} - 2
                    \boldsymbol{\mu}_{0}^{\mathsf{T}}
                    \boldsymbol{\Lambda}_{0}^{-1}
                    \boldsymbol{\omega}\right)\right) \end{aligned}$$
 



 이제 $\boldsymbol{\Lambda}_{m} = \left(
                    \boldsymbol{X}^{\mathsf{T}} \boldsymbol{X} +
                    \boldsymbol{\Lambda}_{0}^{-1} \right)^{-1}$ ,
 $\boldsymbol{\mu}_{m} = \boldsymbol{\Lambda}_{m} \left(
                    \boldsymbol{X}^{\mathsf{T}}\boldsymbol{y} +
                    \boldsymbol\Lambda_{0}^{-1} \boldsymbol\mu_{0} \right)$으로
 정의하고 위 식을 다시 정리하면 다음과
 같다($\boldsymbol{\omega}$를 포함하지 않는 항은 위에서와
 마찬가지로 생략했다).
 



 $$\begin{aligned}<br/>p \left ( \boldsymbol{\omega} |
                    \boldsymbol{X} , \boldsymbol{y} \right) &amp; \propto \exp
                    \left(- \frac{1}{2} \left(\boldsymbol{\omega} -
                    \boldsymbol{\mu} _{m} \right)^{\mathsf{T}}
                    \boldsymbol{\Lambda}_{m}^{-1} \left(\boldsymbol{\omega} -
                    \boldsymbol{\mu}_{m} \right) + \frac{1}{2}
                    \boldsymbol{\mu}_{m}^{\mathsf{T}}
                    \boldsymbol{\Lambda}_{m}^{\mathsf{T}} \boldsymbol{\mu}_{m}
                    \right) \\ &amp; \propto \exp \left(- \frac{1}{2}
                    \left(\boldsymbol{\omega} - \boldsymbol{\mu} _{m}
                    \right)^{\mathsf{T}} \boldsymbol{\Lambda}_{m}^{-1}
                    \left(\boldsymbol{\omega} - \boldsymbol{\mu}_{m} \right)
                    \right) \\ \end{aligned}$$
 



 마지막 식을 통해 다음과 같은 함의를 얻을 수 있다.
 


- 보통 $\boldsymbol{\mu}_{0}$은 $0$으로 설정하는데
 $\boldsymbol{\Lambda}_{0} = \frac {1}{\alpha}
                      \boldsymbol{I}$로 설정하면 추정치 $\mu_{m}$은 frequentist
 선형 회귀에서 decay 페널티를 $\alpha
                      \boldsymbol{\omega}^{\mathsf{T}}\boldsymbol{\omega}$로 준
 추정치와 같게 된다.



 하지만 frequentist 선형 회귀와는 다음과 같은 차이가 있다.
 


- 베이지안 추정에서는 $\alpha$를 0으로 두면 추정치가
 정의되지 않음.
	- 이것은 우리가 무한하게 넓은 범위의 prior를 설정할 수
	 없다는 의미로 볼 수 있다.
- 베이지안 추정은 $\mu_{m}$의 점추정만 제공하지 않고 모든
 분포를 제공.




---


# 5\.6\.1 Maximum *A Posteriori* (MAP) Estimation



 정석적인 방법은 베이지안 사후 분포를 이용하여 파라미터에
 대한 예측을 하는 것이다. 하지만 베이지안 사후 분포를 통해
 작업을 하는 것이 쉽지 않은 모델인 경우, 점추정을 하는 것이
 다루기 쉬운 근사값을 제공하여 유용할 때가 있다.
 



 그렇다고 해서 단순히 ML 추정치를 써야 하는 것인가 할 수
 있지만 점추정을 할 때에도 prior 신념이 개입할 수 있도록 하는
 방법이 있다.
 



 그 중 한 가지 방법이 **maximum a posteriori** (MAP) 점
 추정이다.
 



 MAP 추정은 사후 확률(연속형인 경우 확률 밀도)을 극대화하는
 모수 값을 선택한다. 즉, 다음 식과 같다.
 



 $$ \boldsymbol{ \theta }_{\text{MAP}} =
                    \underset{\boldsymbol{\theta}}{\text{arg max}} p \left(
                    \boldsymbol{\theta} | \boldsymbol{x} \right) = \underset
                    {\boldsymbol{\theta}}{\text{arg max}} \log p \left(
                    \boldsymbol{x} | \boldsymbol{\theta} \right) + \log p \left(
                    \boldsymbol{\theta} \right) $$
 



 가장 우측 변의 첫번째 항은 일반적인 로그 우도 항이고 그 다음
 항은 사전 분포 항이다.
 



 만약에 $\mathcal{N} \left( \boldsymbol{\omega} ;
                    \boldsymbol{0} , \frac{1}{\lambda} \boldsymbol{I}^{2}
                    \right)$의 가우시안 사전 분포를 가정한 weight가
 $\boldsymbol{w}$인 선형 회귀 모형을 위의 식으로 추정한다고
 해보자. 그러면 로그\-사전 분포 항은 $\lambda \boldsymbol{
                    \omega} ^{\mathsf{T}} \boldsymbol{\omega}$ weight decay
 페널티와 비슷한 어떤 항과 $\boldsymbol{\omega}$에 영향을
 받지 않고 학습에 영향을 주지 않는 어떤 항을 더한 것에 비례할
 것이다.
 



 즉, weight의 가우시안 사전 분포를 가정한 베이지안 추정은
 weight decay와 일맥상통한다.
 



 full 베이지안 추정이 그런 것처럼 MAP 베이지안 추정도
 training data에 없지만 사전 신념을 통해 얻을 수 있는 정보의
 방향으로 추정을 이끌 수 있다는 장점이 있다.
 



 그러한 추가 정보는 MAP 점추정을 ML 추정치보다 분산이 작게
 만드는 장점이 있지만 편의가 증가할 수 있다는 비용이
 존재한다.
 



 최우 추정 학습에 weight decay로 regularize한 추정이 MAP
 추정을 한 것으로 해석될 수 있는 것처럼 여러 regulized 추정
 방법이 MAP 추정으로 해석될 수 있다. 하지만 정규화가 목적
 함수에 $\log p ( \boldsymbol{\theta})$와 같이 데이터에
 영향을 받지 않는 정규화 항을 더하는 형식일 때만 그러한
 해석이 가능하다.
 




---



 cf) 엄밀하지 않은 메모  
MAP와 full Bayesian 차이 :
 


- MAP :  
$p\left(\boldsymbol{\theta}|data\right) =
                      p\left(data|\boldsymbol{\theta}\right) *
                      p\left(\boldsymbol{\theta}\right) / p\left(data\right)$를
 극대화하는 $\boldsymbol{\theta}$를 찾자. 그리고
 prediction을 할 거면 그 $\boldsymbol{\theta}$를 가지고
 $p\left(y | \boldsymbol{\theta_{\text{MAP}}} \right)$를
 구하자.
- Bayesian :  
아래처럼 $\boldsymbol{\theta}$에 대해
 marginalize해서 예측하자.  
$$\begin{aligned}
                      p\left(y|data\right) &amp; = \int p\left(y |
                      \boldsymbol{\theta}\right) * p\left(\boldsymbol{\theta}|
                      data\right) d\boldsymbol{\theta} \\ &amp; = \int p
                      \left(y| \boldsymbol{\theta}\right) *
                      p\left(data|\boldsymbol{\theta}\right) *
                      p\left(\boldsymbol{\theta}\right) / p\left(data\right)
                      d\boldsymbol{\theta} \\ \end{aligned}$$
