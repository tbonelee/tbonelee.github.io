---
layout: post
title: "[Deep Learning Book] 5.4"
date: "2021-10-06 00:29:52 +0900"
categories:
  - 머신러닝
---
# 5\.4 Estimators, Bias and Variance



 통계학의 도움을 통해 training set에서 학습한 모델을
 generalize하는 데 큰 도움을 얻을 수 있다.
 



 모수 추정(parameter estimation), 편의(bias), 분산(variance)
 등을 통해 generalization, underfitting, overfitting의 개념을
 formal하게 특징지어볼 수 있다.
 




---


## 5\.4\.1 Point Estimation(점 추정)


- 점 추정은 관심있는 무언가의 '가장 나은'
 prediction을 얻는 것이다.
	- 관심있는 것은 parametric 모형에서 하나의 parameter일
	 수도 있고 parameter의 벡터일 수도 있고(선형 회귀이면
	 weights), 그냥 함수 전체일 수도 있다.
- parameter의 estimate값과 true값을 구분하기 위해 보통
 파라미터 $\theta$에 대한 점 추정을 $\hat{\theta}$으로
 표기한다.
- ${ x^{(1)} ,..., x ^{(m)} }$을 $m$개의 i.i.d. data
 points라고 했을 때 **point estimator** 또는
 **statistic**은 data의 다음과 같은 어떠한
 함수도 될 수 있다.
	- $$\hat{\theta}_{m} = g( x^{(1)} ,..., x^{(m)} )$$
	- 추정량이 되기 위해서 $g$가 true $\theta$에 가까운
	 특정한 값이나 $\theta$가 가질 수 있는 특정 범위의 값을
	 가져야 할 필요는 없다. (매우 general한 개념으로
	 추정량을 만드는 사람의 재량에 달려 있음)
		- 물론 true $\theta$에 가까운 추정량이 좋은
		 추정량이라 할 수 있을 것
- 여기서부터는 frequentist의 관점에 따라 true parameter
 $\theta$가 고정되어 있지만 알지 못하는 값이라 가정.
- 또한 $\hat{\theta}$는 data의 함수이고, data는 random
 process에서 뽑은 것이므로 $\hat{\theta}$도 확률 변수이다.
- 점 추정 중에서 특별히 input과 target 변수의 관계를
 추정하는 것과 관련된 것을 function estimator라고 하는데
 아래에서 간략히 보자.


### Function Estimation


- input vector $x$를 통해 $y$라는 변수를 추정하려 하는 상황
- 함수 $f(x)$가 $y$와 $x$의 대략적인 관계를 설명할 수 있다고
 가정.
	- ex) $y = f(x) + \epsilon$이고 $\epsilon$은 $y$중 $x$로
	 설명되지 않는 부분
- function estimation은 $f$를 모델로 approximating하거나
 $\hat{f}$를 추정하는 것
	- 결국 $\theta$라는 파라미터를 추정하는 것과 같고,
	 function estimator $\hat{f}$는 함수 공간에서
	 점추정량일 뿐




---



 여기서부터 점 추정량의 특성과 특성이 알려주는 추정량에 대한
 정보를 볼 것이다.
 


## 5\.4\.2 Bias


- 추정량의 편의는 다음과 같이 정의
	- $$\text{ bias}( \hat{\theta}_{m} ) = \mathbb{E} (
                          \hat{\theta}_{m} ) - \theta $$
- $\text{bias}( \hat{\theta}_{m} )= 0$이면 추정량
 $\hat{\theta}_{m}$는 **unbiased**라고 하며,
 $\mathbb{E} ( \hat{\theta}_{m}) = \theta$와 동치이다.
- $\text{lim}_{m \rightarrow \infty} \text{bias}
                      (\hat{\theta}_{m} ) = 0$이면
 **asymptotically unbiased**라고 하며,
 $\text{lim} _{m \rightarrow \infty} \mathbb{E}
                      (\hat{\theta}_{m}) = \theta$와 동치이다.
- unbiased 추정량을 추구하는 것은 좋지만 그게 항상
 '최선'은 아니다. (다른 성질도 있기 때문)


## 5\.4\.3 Variance and Standard Error


- 추정량의 **variance**는 다음과 같다.
	- $$\text{Var}(\hat{\theta})$$
- **standard error**는 추정량 variance의
 제곱근이고 ${SE}(\hat{\theta})$로 표기.
- 두 지표를 통해 dataset을 가정된 data generating
 process에서 resample했을 때 추정량이 얼마나 다르게
 나타날지 추측해 볼 수 있다.
- mean값의 standard error는 다음과 같다.
	- $${SE}(\hat{\mu}_{m}) = \sqrt{Var [
                          \frac{1}{m}\sum_{i=1}^{m}x^{(i)}] } =
                          \frac{\sigma}{\sqrt{m}}$$
	- $\sigma ^{2}$는 $x^{i}$의 true variance인데 알 수 없기
	 때문에 $\sigma$에 대한 추정량을 통해 추정되곤 한다.
		- 하지만 표본 분산의 제곱근이나 분산의 불편 추정량의
		 제곱근 모두 표준 편차의 불편 추정량이 아니다.
		- 둘 모두 true S.D.를 과소추정하는 경향이 존재하고,
		 그나마 분산의 불편 추정량에 제곱근을 취한 것이 덜
		 과소추정.
		- $m$이 크면 그래도 쓸 만하다.
	- 머신러닝 관점에서 generalization error를 test set의
	 error의 sample mean으로 추정하기 때문에 mean의
	 standard error는 중요할 수 밖에 없다.
		- CLT를 통해 sample mean이 정규 분포에 근사할 것임을
		 알고 standard error를 이용하여 구한 인터벌에 true
		 기댓값이 존재할 확률을 구할 수 있다.
		- 머신 러닝 실험에서 알고리즘 A의 에러 신뢰 구간
		 상방이 알고리즘 B의 에러 신뢰 구간 하방보다 낮으면
		 A가 B보다 낮다는 식의 표현을 자주 쓴다(같은 확률
		 하에)


## 5\.4\.4 Trading off Bias and Variance to Minimize Mean Squared
 Error


- Bias는 함수나 파라미터의 true value로부터의 기대
 deviation을 측정하고, Variance는 추정량의 기댓값으로부터
 data의 임의 샘플링에서 계산한 추정량까지의 deviation을
 측정한다.
- bias\-variance trade\-off 하에서 강우월한 모델이 없을 때
 **mean squared error(MSE)**를 통해 비교할 수
 있다.
	- $$\begin{aligned}\text{MSE} &amp; = \mathbb{E} [ (
                          \hat{\theta}_{m} - \theta)^{2} ] \ &amp; =
                          \text{Bias}(\hat{\theta}_{m})^{2} + \text{Var}
                          (\hat{\theta}_{m}) \end{aligned}$$
	- MSE는 파라미터의 참값과 추정량 사이의 overall 편차를
	 squared error를 통해 측정해준다.
	- ![](/public/img/머신러닝/remote/blog.kakaocdn.net/dn/t0ymD/btrgXpJqvOX/H2zxPatu7NhlPLci47wuHK/img-2.png)
	- capacity가 증가할 때 bias는 감소하고 variance는
	 증가하는 경향.
		- 모형의 capacity가 작으면 bias는 내재적이라 할 수
		 있다(ex. 선형 모형을 통해 실제에 가까운 모형을
		 학습하기는 어렵다)
		- 모형의 capacity가 크면 모형은 학습될 때마다
		 training\-set에 특화되기 쉽다. 따라서 모형의 분산이
		 커진다고 볼 수 있음
	- 최적 capacity보다 크면 overfitting, 작으면
	 underfitting


## 5\.4\.5 Consistency


- 일치성(weak consistency)의 성립은 다음 식의 성립을 의미.
	- $${plim}_{m \rightarrow \infty} \hat{\theta}_{m} =
                          \theta$$
	- ${plim}$은 확률 수렴으로, 임의의 $\epsilon &gt; 0$에
	 대해 $m \rightarrow \infty$이면 $P( | \hat{\theta}_{m}
                          - \theta | &gt; \epsilon ) \rightarrow 0$이어야 함을
	 의미한다.
- 확률 수렴 대신 거의 확실한 수렴(almost surely
 convergence)인 경우 strong consistency라고 한다. (strong
 consistency에 대비해서 weak consistency라 부르기도 함)
	- 확률 변수 $x^{(1)}$, $x^{(2)}$,... 의 수열이 값 $x$에
	 수렴하는 것은 $p({lim} _{m \rightarrow \infty} x^{(m)}
                          = x) = 1$임을 의미
- 일치성이 성립하면 표본 크기가 늘수록 추정량의 편의는
 감소한다(일치성 ⇒ 점근적 불편성).
	- (물론 일치성이 성립하면, 표본 크기가 늘어날 수록
	 단순히 편의가 감소하는 것이 아니라(기댓값에 대한 것)
	 추정량 '자체'(기댓값이 아님)가 모수에
	 가까워짐)
	- 하지만 역은 성립하지 않음(즉, 점근적 불편성이 일치성의
	 충분조건이 아님)
		- ex)
			- $N(x; \mu , \sigma^{2})$의 분포의 dataset이
			 $m$개의 표본 ${x^{(1)} , ... , x^{(m)} }$으로
			 이루어져 있다고 할 때 $\hat{\theta} =
                                  x^{(1)}$로 추정량을 잡는다고 해보자.
			- 그러면 $\mathbb{E} ( \hat{\theta} _{m}) =
                                  \theta$가 성립하여 불편성이 항상 성립한다.
			 하지만 $m \rightarrow \infty$일 때
			 $\hat{\theta}_{m} \rightarrow \theta$가
			 아니므로 일치성은 성립하지 않는다.
