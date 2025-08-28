---
layout: post
title: "[Deep Learning Book] 5.3"
date: "2021-09-19 18:07:57 +0900"
categories:
  - 머신러닝
---
# 5\.3 Hyperparameters and Validation Sets


- **Hyperparameter**를 통해 학습 알고리즘의
 행동을 제어할 수 있다.
	- 하이퍼파라미터는 학습 알고리즘 자체에 의해 조정되지
	 않는다(물론 다른 학습 알고리즘의 하이퍼파라미터를
	 학습할 수는 있다)
	- 다항 회귀 예시에서 하이퍼파라미터는 다항식의 차수
	 하나만 있었고, 이는
	 **capacity** hyperparameter로 작용했다.
	- weight decay의 강도를 조절하는데 쓰였던 $\lambda$값도
	 하이퍼파라미터라고 할 수 있다.
	- 하이퍼파라미터를 최적화 문제를 통해 정하기도 하지만
	 보통은 최적화 솔루션을 구하는 게 쉽지 않다. 게다가
	 training set에서 하이퍼파라미터를 학습하는 것은 보통
	 적절하지 않다.
		- 특히 모델 capacity를 제어하는 하이퍼파라미터의
		 경우가 그렇다.
		- training set에서 학습하면 최적의 하이퍼파라미터는
		 최대의 모델 capacity를 선택하는 것이다.
			- ex) 최대의 다항 차수와 $\lambda = 0$의 weight
			 decay


(보통의 하이퍼파라미터 : 몇 번 사이클 돌릴 것인지 etc.)


- 이러한 문제를 해결하기 위해 training 알고리즘이 관찰하지
 못하는 **validation set**의 example이
 필요하다.
	- 앞서서 training set과 동일한 분포에서 나왔지만
	 독립적으로 뽑힌 test set을 통해 알고리즘의 퍼포먼스를
	 체크한다고 했다.
	- 여기서 중요한 것은 test example을 이용해서
	 하이퍼파라미터를 포함한 어떠한 모형 선택을 결정해서는
	 안 된다는 것이다.
	- 따라서 validation set은 항상 training data에서
	 만들어야 한다.
	- validation set은 학습 때 사용하지 않는다. 보통
	 80퍼센트의 training set을 모델 학습에, 나머지
	 20퍼센트는 validation set으로 하이퍼파라미터 학습에
	 사용된다.
	- validation set은 하이퍼파라미터를 훈련하는데
	 사용되므로 validation set error는 generalization
	 error를 과소 추정하게 된다. 물론 보통 training
	 error보다는 덜 과소 추정한다.
	- 모든 하이퍼파라미터 최적화가 끝난 후 test set을 이용해
	 generalization error가 추정된다.
- 현실에서는 동일한 test set이 성능 측정에 사용되고,
 학계에서 이를 목표로 삼다 보니 해당 test set에 과적합되는
 경우가 많다.
	- 그렇게 되면 현실 반영을 못하는 경우가 많지만 다행히도
	 새로운 벤치마크 dataset이 지속적으로 제공되고 있다.
	- 하면 안 되는 것을 하고 있다는 얘기..


### 5\.3\.1 Cross\-Validation


- dataset을 고정된 training set과 고정된 test set으로 나누는
 것 때문에 test set의 크기가 작게 된다면 문제가 될 수 있다.
	- test set이 작은 것은 추정된 평균 test error가
	 통계적으로 uncertainty를 가짐을 의미하므로, 알고리즘
	 A가 알고리즘 B보다 주어진 일을 더 잘 해내는지 구별하기
	 어려워진다.
- 그런 경우에 dataset을 랜덤하게 training set과 data set으로
 나누고 test error를 추정하는 것을 여러 번 반복하는 대안을
 선택한다.
	- k\-fold cross\-validation 프로시져가 그러한 경우이다.
	- k번의 시행에서 dataset을 training set과 data set으로
	 나누는데 서로 다른 시행에서 test set이 겹치지 않게
	 한다.
	- 그 후 k번 시행의 test error의 평균을 test error의
	 추정값으로 사용한다.
	- 평균 test error 추정량의 분산에 대한 불편 추정량이
	 없다는 것이 문제지만 근사값이 사용된다.



 (test set을 골랐는데 그게 운 좋게 결과가 좋은 것일 수도
 있다. 그러니까 다른 나머지도 test set으로 골라서 해보는 게
 좋다. (정석))
 


(그런데 빡세서 그렇게 하지는 않음)
