---
layout: post
title: "[Deep learning book] 5.5 Maximum Likelihood Estimation"
date: "2021-10-08 00:29:51 +0900"
categories:
  - 머신러닝
---
앞에서는 추정량에 대한 함수를 guess하고, 그 추정량의 bias와
 variance와 같은 성질들을 분석했었다.
 



 그러는 대신 특정한 원칙을 통해 여러 모형들에서 좋은 추정량
 함수를 구해보자.
 


- Maximum likelihood principle이 그러한 원칙 중 가장 많이
 쓰이는 것
- MLE의 가정
	- 알려지지 않은 true data generating distribution
	 $p_{\text{data}} ( \boldsymbol{x})$에서 독립적으로
	 추출된 $m$개 example의 집합, $\mathbb{X} = { x^{(1)}
                          ,..., x^{(m)} }$을 가정
	- $p_{\text{model}} (\boldsymbol{x}; \theta)$는 임의의
	 벡터 $x$를 true probability $p_{\text{data}}(x)$를
	 추정하는 실수로 맵핑한다.
		- $p_{\text{model}} (\boldsymbol{x}; \theta)$
		 family는 확률 분포의 family이고 각 $\theta$마다
		 고유한 확률 분포를 갖는다고 볼 수 있다.
- $\theta$에 대한 maximum likelihood estimator인
 $\theta_{\text{ML}}$은 다음과 같이 정의된다.



 $$<br/>\begin{aligned}<br/>\theta_{\text{ML}} &amp; =
                    \text{argmax}_{\theta} p_{model} ( \mathbb{X}; \theta) \<br/>&amp;
                    = \text{argmax}_{\theta} \prod_{i=1}^{m} p_{\text{model}} (
                    x^{(i)} ; \theta)\<br/>\end{aligned}<br/>$$
 


- 하지만 numerical underflow등 불편한 점이 많으므로
 likelihood식에 로그를 취해 동일한 해를 갖는 새로운
 문제를 만든다. 따라서 $\theta_{\text{ML}}$은 다음과 같이
 쓸 수 있다.
 


	- $$\theta_{\text{ML}} = \text{argmax}_{\theta}
                            \sum_{i=1}^{m}\log p_{\text{model}} (x^{(i)} ;
                            \theta)$$
	- 위 식의 목적식을 $m$으로 나누면 $x$의 실증적 분포
	 $\hat{p}_{\text{data}}$에 대한 기댓값의 식으로
	 표현할 수 있다.
	 
	
	
	
	 \-  
	
	 $$\theta_{\text{ML}} = \text{argmax}_{\theta}
                            \mathbb{E}_{\boldsymbol{x} \sim
                            \hat{p}_{\text{data}}} \log p_{\text{model}}
                            (x;\theta)$$
- maximum likelihood estimation은 training set에 의해
 정의된 실증 분포 $\hat{p}_{\text{data}}$와 모델 분포
 $p_{\text{model}}$ 사이의 격차를 줄이는 것으로 해석할 수
 있다.
 


	- 이 때 격차는 KL divergence로 측정되고 KL
	 divergence는 다음과 같다.
	 
	
	
		- ($\hat{p}_{\text{data}}$가 사후분포,
		 $p_{\text{model}}$가 사전분포)
		 
		
		
		
		 \-  
		
		 $$D_{\text{KL}} = ( \hat{p}_{\text{data}} |
                                p_{\text{model}} ) = \mathbb{E}_{\boldsymbol{x}
                                \sim \hat{p}_{\text{data}}} [ \log
                                \hat{p}_{\text{data}} ( x) - \log
                                p_{\text{model}} (x) ]$$
	- 위 식의 좌변은 오직 data generating process의
	 함수이며 따라서 다음 식만 최소화하면
	 된다($\hat{p}_{\text{data}}$인 사후분포는 데이터로
	 이미 주어졌으므로 바꿀 수 없음)
	 
	
	
	
	 \-  
	
	 $$\mathbb{E} _{\boldsymbol{x} \sim
                            \hat{p}_{\text{data}}} [\text{log} p_{\text{model}}
                            (x) ]$$


- KL divergence의 최소화는 empirical 분포와 true 분포의
 cross\-entropy 최소화와 일치
- 즉, maximum likelihood는 모델 분포가 실증
 분포($\hat{p}_{\text{data}}$)와 일치하도록 하는 것.
	- 이상적으로는 true data generating distribution인
	 $p_{\text{data}}$와 일치하게 하는 것이 좋지만 그
	 분포를 직접 볼 수 없기에..
- NLL의 최소화, cross\-entropy최소화, KL divergence 최소화
 모두 같은 최적 $\theta$를 갖지만 목적식의 값이 다르다.
	- KL divergence의 경우 최솟값이 0이라는 점에서 목적식의
	 값을 눈으로 체크하기에 유리한 면이 있다.


# 5\.5\.1 Conditional Log\-Likelihood and Mean Squared
 Error(조건부 로그 우도와 MSE)


- $\boldsymbol{x}$가 주어진 상황에서 $\boldsymbol{y}$를
 추정하기 위해 조건부 확률 $P(\boldsymbol{Y} |
                      \boldsymbol{X}; \boldsymbol{\theta})$을 추정하는 케이스로
 최우추정법을 일반화할 수 있다.
- $\boldsymbol{X}$가 모든 input, $\boldsymbol{Y}$가 모든
 관찰된 target을 나타낸다 할 때 조건부 최우 추정량은 다음과
 같다.



 $$\theta_{\text{ML}} = \text{argmax}_{\theta}
                    P(\boldsymbol{Y} | \boldsymbol{X}; \theta)$$
 


- example이 모두 i.i.d.라고 가정하면 다음과 같이 분해할 수
 있다.



 $$\theta_{\text{ML}} = \text{argmax}_{\theta} \sum
                    _{i=1}^{m} \log P(y^{(i)} | x^{(i)} ; \theta)$$
 


## linear regression을 maximum likelihood로


- $x$에서 $\hat{y}$를 예측하던 기존의 선형 회귀를 조건부
 분포 $p(y|x)$를 만드는 문제로 볼 수 있다.
	- 많은 training example에서 같은 $x$여도 다른 $y$를 가진
	 것들이 있고, 알고리즘 학습을 통해 해당 $x$하에서 $y$의
	 분포($p(y|x)$)를 알고자 하는 것이다.
- $p(y|x) = \mathcal{N}(y;\hat{y}(x;\omega), \sigma^{2})$로
 정의하고, 함수 $\hat{y}(x; \omega)$는 가우시안 분포의 mean
 예측값이라 하자. 또한 분산 $\sigma^{2}$도 고정된 값이라
 가정한다.
- example이 모두 i.i.d.라고 가정하면 조건부 우도 함수는
 다음과 같다.



 $$<br/>
                    \sum_{i=1}^{m} \log p(y^{(i)} | x^{(i)} ; \theta) \<br/>
                    = - m \log \sigma - \frac{m}{2} \log(2 \pi) - \sum_{i=1}^{m}
                    \frac{|\hat{y}^{(i)} - y^{(i)} |^{2} }{2 \sigma^{2}} \<br/>$$
 


- 그런데 아래와 같이 MSE와 마지막 항을 일치시킬 수 있고,
 $\omega$에 대해 로그 우도함수를 극대화 시키는 것은 곧
 MSE를 극소화하는 것과 같음을 알 수 있다.
 



 \-  

 $$\text{MSE}_{\text{train}} = \frac{1}{m} \sum_{i=1}^{m}
                        | \hat{y}^{(i)} - y^{(i)} |^{2}$$
- 이를 통해 MSE 극소화의 사용을 어느 정도 정당화할 수
 있다.


# 5\.5\.2 Properties of Maximum Likelihood


- the best estimator asymptotically, as the number of
 examples $m \rightarrow \infty$, in terms of its rate of
 convergence as $m$ increases
- 다음 조건 하에 일치성 성립
	- The true distribution $p_{\text{data}}$ must lie
	 within the model family $p_{\text{model}} (\cdot ;
                          \theta)$. Otherwise, no estimator can recover
	 $p_{\text{data}}$
	- The true distribution $p_{\text{data}}$ must
	 correspond to exactly one value of $\theta$.
	 Ohterwise, maximum likelihood can recover the correct
	 $p_{\text{data}}$, but will not be able to determine
	 which value of $\theta$ was used by the data
	 generating processing.
- **statictic efficiency**의 측면에서는 같은
 일치 추정량이라도 차이가 존재할 수 있다.
	- 한 샘플 크기 $m$에서 한 일치 추정량이 다른 것보다 더
	 낮은 generalization error를 가질 수 있고, 뒤집어
	 말하면 어느 수준의 generalization error를 얻기
	 위해서는 더 적은 example만 있어야 할 수 있다.
- 이러한 statistical efficiency는 함수의 값이 아닌
 파라미터의 값을 추정하는
 **parametric case**에서 연구되고, expected
 mean squared error of parameter를 통해 추정된 파라미터와
 true값 사이의 격차를 측정한다.
- 여기서 기댓값은 $m$개의 training 샘플로 학습되고,
- parametric mean squared는 $m$이 증가할수록 감소한다.
- 충분히 큰 $m$에 대해서는 Cramer\-Rao lower bound를 통해
 MLE보다 더 작은 MSE를 갖는 추정량이 없음이 밝혀져 있다.
- 이러한 이유로 머신러닝에서 maximum likelihood 추정량이
 종종 선호되고,
- example 갯수가 작아서 overfitting이 우려되는 경우 weight
 decay와 같은 regularization 전략을 통해, variance가 작은
 biased version의 최우추정량을 사용한다.
