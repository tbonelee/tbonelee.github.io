---
layout: post
title: "[Deep learning book] 5.1 ~ 5.2"
date: "2021-09-16 03:49:54 +0900"
categories:
  - 머신러닝
---
# Ch.5 Machine Learning Basics


# 5\.1 Learning Algorithms



 머신 러닝 알고리즘은 데이터에서 학습(learn)할 수 있는
 알고리즘.
 


Mitchell(1997\)에서 다음과 같이 말함



> A computer program is said to learn from experience $E$
>  with respect to some class of tasks $T$ and performance
>  measure $P$, if its performance at tasks in $T$, as
>  measured by $P$, improves with experience $E$.


## 5\.1\.1 The Task, $T$


ex) 로봇이 걷게 하고 싶다 하면 '걷는'것이 task


- 머신 러닝 tasks는 머신 러닝 시스템이 어떻게
 **example**을 프로세스해야 하는지의 방식으로
 설명되곤 한다.
- example은 우리가 머신러닝 시스템이 프로세스(처리)하기
 바라는 어떤 object나 event로부터 양적으로 측정된
 **features**의 모음이다.
	- example은 보통 각 요소 $x_{i}$가 다른 feature인 벡터
	 $\mathbf{x} \in \mathbb{R}^{n}$로 나타낸다.  
	예를
	 들어 이미지의 feature는 이미지 내의 픽셀값이다.


많은 종류의 task를 머신러닝을 통해 해결할 수 있다.


가장 흔한 머신러닝 task는 다음과 같다.


### Classification(분류):



 어떤 인풋이 $k$개의 카테고리 중 어디에 속하는지 명시하도록
 컴퓨터 프로그램에게 지시한다.
 



 이를 위해 learning algorithm은 $f: \mathbb{R}^{n}
                    \rightarrow {1, ..., k}$을 만들어내야 한다.
 



 $y = f(\mathbf{x})$이면 모형은 벡터 $\mathbf{x}$로 설명되는
 인풋을 숫자 코드 $y$가 나타내는 카테고리에 할당한다.
 



 꼭 하나의 카테고리에 할당하지 않고 여러 클래스에 대한
 확률분포로 결과를 나타내기도 한다.
 


ex)


- object recognition : 이미지를 인풋, 분류에 해당하는 숫자
 코드를 아웃풋으로 함. 이미지는 보통 픽셀 밝기 값으로
 설명됨
- 얼굴 인식


### Classification with missing inputs:



 인풋 벡터의 모든 관측값이 항상 제공된다고 보장할 수 없다.
 



 관측되지 않은 값이 없다면 학습 알고리즘은 벡터를 카테고리
 결과 값으로 맵핑하는 '하나의' 함수만 정의하면 된다.
 



 그런데 몇몇 인풋이 없다면 학습 알고리즘은 하나의 분류 함수가
 아니라 한 set의 함수를 학습해야 한다.
 



 각 함수는 $\mathbf{x}$를 사라진 인풋의 각각 다른 부분집합에
 대응된다.
 



 이러한 상황은 검사 비용이 큰 의학 진단에서 자주 볼 수 있다.
 



 이와 같은 대규모의 함수 set을 효과적으로 정의하는 방법은
 모든 관련된 변수에 대한 확률 분포를 학습하는 것이다.
 



 $n$개의 인풋 변수를 가지고 모든 가능한 사라진 인풋 set에
 대해 필요한 $2^{n}$개의 서로 다른 분류 함수를 만들 수 있다.
 하지만 결합 확률 분포를 설명하는 단 하나의 함수만 학습하면
 된다.
 


### Regression:



 학습 알고리즘이 $f :\mathbb{R}^{n} \rightarrow \mathbb{R}$를
 뱉어내야 한다.
 


분류랑 비슷하지만 아웃풋의 포맷만 조금 다르다.



 ex) 보험 가입자가 나중에 청구하게 될 금액의 기댓값을 예측,
 증권 미래 가격 예측 같은 것들에 사용됨. 이런 것들은 알고리즘
 트레이딩에도 사용된다.
 


### Transcription



 여기서 머신 러닝 시스템은 구조화되지 않은 데이터를 관찰해서
 discrete, textual 형태의 데이터로 변환해야 한다.
 



 ex) 광학 문자 인식(optical character recognition), speech
 recognition
 


### Machine translation:



 특정 언어의 심볼의 sequence를 인풋으로 받아서 다른 언어의
 심볼 sequence로 변환해야 한다.
 


### Structured output:


아웃풋이 요소 간의 관계를 나타내는 벡터인 경우.



 위의 transcription이나 translation은 물론 다른 task도
 포함한다.
 



 ex) parsing : 자연어 문장을 문법적 구조를 설명하는 트리로
 맵핑하고, 트리의 노드를 동사, 명사, 부사 등등으로 태깅하는
 것.
 



 ex2\) 이미지의 각 픽셀을 특정 카테고리로 분류. 풍경 사진의
 길을 특정 장소로 annotate할 수도 있음.
 



 ex3\) image captioning : 이미지를 보고 여러 단어로 구성된
 하나의 문장으로 나타내도록 함.
 



 output의 값들이 서로 밀접하게 상호 연관되어있기 때문에
 structured output이라 한다.
 


### Anomaly detection:



 이벤트나 object의 set을 걸러내면서 일반적이지 않거나
 전형적이지 않은 것들에 플래그를 붙이는 일.
 



 ex) 신용 카드 위조 감지 : 사용자의 구매 습관을 모델링해서
 카드의 잘못된 사용을 감지. 도둑의 결제 패턴은 기존 사용자의
 구매 확률 분포에서 보기 힘들 것.
 


### Synthesis and sampling:


training 데이터와 유사한 새 예시들을 생성해야 한다.


ex) 게임 내 풍경 데이터 합성.



 ex2\) 글로 쓰여진 문장 input으로 받아서 스피치 버전의 output
 생성
 


### Imputation of missing values:



 몇몇 엔트리 $x_{i}$가 없는 example $\mathbf{x} \in
                    \mathbb{n}$을 받아서 missing 엔트리를 예측해야 한다.
 


### Denoising:



*clean example* $\mathbf{x} \in \mathbb{R}^{n}$\_을
 알려지지 않은 corruption 프로세스를 통해
 *corrupted example* $\tilde{\mathbf{x}} \in
                    \mathbb{R}^{n}$\_로 만든 것을 input으로 받는다.
 



 learner는 corrupted version $\tilde{\mathbf{x}}$에서 clean
 example $\mathbf{x}$를 예측해야 하거나, 보다 일반적으로는
 조건부 확률 분포 $p(\mathbf{x} | \tilde{\mathbf{x}})$을
 예측해야 한다.
 


### Density estimation or probability mass function estimation:



 함수 $p_{\text{mode}} : \mathbb{R}^{n} \rightarrow
                    \mathbb{R}$을 학습해야 한다.
 



 $p_{\text{mode}}(\mathbf{x})$는 example을 가져온 space상에서
 $\mathbf{x}$가 연속형이라면 p.d.f., 이산형이라면 p.m.f.로 볼
 수 있다.
 



 이를 잘 수행하기 위해서 알고리즘은 관찰한 데이터의 구조를
 학습해야 한다. (잘 수행한다는 게 뭔지는 performance measure
 $P$를 논할 때 볼 것이다)
 



 output을 통해 다른 task를 해결할 수 있다. (물론 기계적
 한계때문에 모든 것을 해결할 수 있는 것은 아니다..)
 


## 5\.1\.2 The Performance Measure, $P$


시스템이 수행하는 task $T$의 퍼포먼스를 측정.



 classification나 classification with missing inputs,
 transcription에서는 보통 모형의 **accuracy**를
 측정한다. accuracy는 모형이 정확한 output을 만들어낸
 example의 비율이다.  
아니면
 **error rate**를 측정해서 동일한 정보를 얻을
 수도 있다. error rate는 example에서 모형이 만들어낸 틀린
 output의 비율이다.  
종종 error rate를 0\-1 loss의
 기댓값이라 부르기도 한다. 특정 example의 0\-1 loss는 맞게
 분류되었으면 0, 틀리게 분류되었으면 1이다.  
밀도
 추정(density estimation)같은 것에는 accuratcy, erro rate,
 0\-1 loss 같은 것은 적절하지 못하므로 연속형의 다른 지표가
 필요하다. 보통은 몇몇 example에 모형이 부여하는 평균
 로그\-확률값을 보고하게 하는 것을 활용한다.
 



 머신 러닝 알고리즘의 성능을 보려면 알고리즘이 학습하지 않은
 데이터에서 측정해야 한다. 그래야 실제 세계에서의 성능을 알
 수 있다.  
따라서 데이터의 **test set**을
 머신 러닝 시스템을 훈련시키는 데이터와 분리한 후 이를
 사용해서 성능을 측정한다.
 



 성능 측정의 기준을 어떤 것으로 삼을지 정하는 것이 어려운
 경우도 많다.  
예를 들어 transcribing을 할 때 전체
 sequence를 변환하는 것의 accuracy를 척도로 삼을지, 아니면
 sequence의 일부분의 정확성을 보장하여 보다 정밀한
 performance를 척도로 삼을지 쉽게 정할 수 없다.  
또한
 회귀 task에서는 중간 사이즈의 실수를 많이하는 것과 큰
 사이즈의 실수를 가끔 하는 것 중 무엇을 더 안 좋게 볼지
 단순하게 결정할 수 없다.
 



 성능 측정 지표의 측정 자체에 어려움이 있을 수도 있다.  
대개의
 확률 모형은 암묵적으로만 확률 분포를 나타낸다. 많은 모형에서
 해당 space의 특정 지점에 할당된 실제 확률 값을 계산하는 것은
 매우 어렵다.  
이런 경우 디자인 목적에 걸맞는 대안적인
 기준을 고안하거나 목표하는 기준을 잘 근사한 것을 고안해내야
 한다.
 


## 5\.1\.3 The Experience, $E$



 머신 러닝 알고리즘은 learning process에서 허용되는
 experience가 어떤 종류인지에 따라
 **unsupervised**와
 **supervised**로 나눌 수 있다.
 



 이 책에서 배우는 대부분의 알고리즘은 전체
 **dataset**을 experience하도록 허용된 것으로
 이해할 수 있다. dataset은 여러 example의 컬렉션이다.
 example은 **data point**로 불리기도 한다.
 



**Unsupervised learning algorithms(비지도 학습
 알고리즘)**은 여러 feature를 가지고 있는 dataset을 experience하고 해당
 dataset 구조의 유용한 특성을 학습한다. 딥러닝의 맥락에서는
 보통 dataset을 생성한 전체 확률 분포를 학습하고자 한다. 이는
 밀도 추정 같이 명시적으로 이루어질 수도 있고 synthesis나
 denoising처럼 암묵적으로 이루어질 수도 있다.  
Clustering과
 같은 또 다른 비지도 학습 알고리즘은 dataset을 비슷한
 example의 cluster로 나눈다.
 



**Supervised learning algorithms**은 feature를
 가지고 있는 dataset을 experience하는 것은 같지만 각
 example은 **label**이나
 **target**과 연관되어 있다.
 



 단순히 말하면, 비지도학습은 random vector $\mathbf{x}$의
 여러 exmaple을 관찰한 후 확률 분포 $p(\mathbf{x})$을
 암묵적이든 명시적이든 학습하거나 해당 분포의 흥미로운 특성을
 학습하려 하는 것이다. 반면 지도학습은 random vector
 $\mathbf{x}$와 연관된 값이나 벡터 $\mathbf{y}$를 받아서
 $\mathbf{x}$에서 $\mathbf{y}$를 예측하는 방법(보통은
 $p(\mathbf{y} | \mathbf{x})$의 추정)을 학습하고자 한다.  
(비조건부
 확률과 조건부 확률의 차이?)
 


이러한 구분은 엄밀한 것은 아니니 주의하자



**reinforcement learning(강화 학습)**과 같은
 기계 학습 알고리즘은 고정된 dataset을 그냥 experience하지
 않고 피드백을 받으며 환경과 상호작용한다.
 


## 5\.1\.4 Example: Linear Regression


계량경제학에서 공부한 내용과 동일


# 5\.2 Capacity, Overfitting and Underfitting


- 머신 러닝의 주요 도전 과제는 기존에 training에 사용한
 인풋이 아니라
 *새로운, 기존에 관찰하지 않은* 인풋을 가지고
 performance를 보여줘야 한다는 것이다.
 


	- 기존에 관찰하지 않은 input을 가지고 성능을 보여주는
	 능력을 **generalization**이라고 한다.
	- training set에서 측정된 에러를
	 **training error**라 하고, 이를
	 줄여나갈 것이다. 이는 단순히 최적화 문제를 푸는 것과
	 같다.
	- 하지만 머신 러닝이 단순한 최적화와 구별되는 것은
	 **generalization error(test error)**
	 또한 줄여야 한다는 것이다.
	- generalization error는 새 input에서의 에러
	 기댓값으로 정의된다.
	- 해당 기댓값은 시스템이 실제 상황에서 마주칠 것으로
	 기대되는 input의 분포로부터 얻은 각기 다른 가능한
	 input들로부터 얻어진다.
	- 일반적으로 generalization error는 training set과
	 구별되어서 수집된 example의 test set에서의 퍼포먼스
	 결과를 측정하여 추정된다.
	- 선형 회귀 예시에서 training은 다음의 training
	 error를 최소화함으로써 이루어졌다.
	- $$\frac{1}{m^{(train)}} \parallel
                            \mathbf{X}^{(train)} \omega - \mathbf{y}^{(train)}
                            \parallel ^{2}_{2}$$
	- 하지만 실제로 신경 쓰는 것은 다음의 test error이다.
	- $$\frac{1}{m^{(test)}} \parallel \mathbf{X}^{(test)}
                            \omega - \mathbf{y}^{(test)} \parallel ^{2}_{2}$$


- training set으로 훈련시키면서 어떻게 test error를
 줄일지는 **statistical learning theory**를
 통해 힌트를 얻을 수 있다.
 


	- train data와 test data는
	 **data generating process(DGP)**의
	 dataset의 확률 분포에 따라 생성된다.
	- 보통 **i.i.d 가정**을 통해 각 dataset이
	 서로 독립이고 동일한 분포를 가졌다고 가정한다.
	- 이러한 가정을 통해 여러 example이 아니라 하나의 단일한
	 example에서의 확률 분포를 통해 DGP를 설명할 수 있다.
	- 공유된 분포를
	 **data generating distribution**,
	 $p_{data}$로 부른다.
	- 위의 확률적 프레임과 i.i.d 가정을 통해 training
	 error와 test error의 관계를 수리적으로 배울 수 있다.
	- 우선 확률적으로 선택된 모형에서 기대 training error와
	 기대 test error의 값은 같다.
		- 확률 분포 $p(\mathbf{x} , y)$에서 반복적으로
		 표본을 추출하여 train set과 test set을 생성했다고
		 하자. 고정된 $\omega$에서 둘의 기대 error는 같은
		 dataset 샘플링 프로세스에서 형성되었기 때문에 같을
		 수 밖에 없다.
	- 물론 머신 러닝 알고리즘에서는 사전적으로 파라미터를
	 고정하고 두 dataset을 샘플링하지 않는다. 먼저 training
	 set을 고르고 training set을 최소화하는 파라미터를
	 정하고, 그 다음 test set을 샘플링한다. 그러면 기대
	 test error는 기대 training error와 같거나 더 크다.
- 따라서 머신 러닝 알고리즘의 성능을 결정하는 요소는
 다음과 같게 된다.
 


	1. training error의 최소화
	2. training error와 test error의 gap 최소화
	- 이 두 요소는 머신 러닝의 두 주요한 과제인
	 **underfitting**과
	 **overfitting**과 연결된다.
		- underfitting은 모형이 training set에서 충분히 낮은
		 error를 만들지 못할 때 발생하고, overfitting은
		 training error와 test error의 gap이 너무 클 때
		 발생한다.
		- 모형이 오버피팅에 가까울지 언더피팅에 가까울지를
		 모형의 **capacity**를 통해 조절할 수
		 있다.
		- 엄밀하지 않게 말하면 모형의 capacity는 다양한
		 종류의 함수에 fit할 수 있는 능력이다.
		- low capacity의 모형은 training set에 fit하기 쉽지
		 않고, high capacity의 모형은 training set의 특성을
		 기억하여 쉽게 overfit하게 되므로 test set에는 잘
		 fit하지 않을 수 있다.
- 학습 알고리즘의 capacity를 조절하는 하나의 방법은
 **hypothesis space**를 결정하는 것이다.
 


	- hypothesis space는 학습 알고리즘이 솔루션으로 선택할
	 수 있는 함수들의 집합이다.
	- ex) linear regression 알고리즘은 모형의 input에 대한
	 모든 선형 함수 집합을 hypothesis space로 가지고
	 있다.
	 
	
	
		- 선형 회귀가 hypothesis space 안에 다항식을 갖게
		 함으로써 모형의 capacity를 증가시킬 수 있다.
	- 머신 러닝 알고리즘은 capacity가 하고자 하는 일의
	 실제 복잡도에 가장 적절한 수준이고, 충분한 training
	 data를 제공받을 때 최적의 성능을 보여준다.
	 
	
	
		- 충분하지 못한 capacity로는 복잡한 일을 풀 수
		 없다.
		- 높은 capacity로는 복잡한 문제를 풀 수 있지만
		 capacity가 필요한 수준보다 높으면 overfit할 수
		 있다.
		- 다음 그래프는 이를 시각적으로 보여준다.
		- ![](/public/img/머신러닝/img/img.png)
		- capacity는 파라미터를 추가하고 feature차수를
		 조절하는 식의 모형 선택을 통해 조절할 수도
		 있지만, 모형에서 사용할 수 있는 함수들을
		 변경해서 조절할 수도 있다.
		- 모형에서 사용할 수 있는 함수들을 통해 결정되는
		 capacity를
		 **representational capacity**라고
		 한다.
		- 하지만 실제로는 사용할 수 있는 함수 family에서
		 최적의 함수를 찾는 것은 매우 어려운 최적화
		 문제를 풀어야 한다.
		- 따라서 실전에서는 최적을 찾기보다 training
		 error를 유의하게 줄여주는 함수를 선택한다.
		- 그렇기 때문에
		 **effective capacity**는
		 representational capacity보다 작을 수 있다.


- 머신 러닝 모형의 generalization을 개선하는 기본적인
 아이디어는 '오캄의 날'이다. 똑같이 설명력이 좋은
 모형이 있다면 가장 '단순한'것을 골라야 한다는
 얘기다. (결국 가능하다면 capacity가 낮은 것으로 고르는
 것이 좋다는 얘기?)
	- statistical learning theory는 다양한 모형 capacity
	 양적 측정 방법을 제시한다.
	 **Vapnik\-Chervonenkis dimension**은
	 binary classifier의 capacity를 측정한다. 여기서 중요한
	 것은 capacity의 양적 측정을 통해 양적인 예측을
	 가능하게 한다는 점이다. statistical learning theroy에
	 따르면 training error와 generalization error의 간극은
	 모형 capacity와 양의 관계를 갖는 quantity에 의해
	 bounded from above하고, training example의 갯수가
	 증가함에 따라 감소한다.
	- 하지만 이 bound가 꽤나 루즈하고 딥 러닝 알고리즘의
	 capacity를 결정하는 것은 쉽지 않기 때문에 딥
	 러닝에서는 잘 쓰이지 않는다.
	- 딥 러닝의 capacity 계산이 어려운 것은 effective
	 capacity가 최적화 알고리즘의 가능성에 제약되고 아직은
	 딥 러닝과 관련된 일반적인 non\-convex 최적화 문제에
	 대한 이론적 이해가 부족하기 때문이다.
- 단순한 함수가 generalize하기에(training error와 test
 error의 갭을 줄이기에) 충분히 복잡한 가설을 설정해서 낮은
 training error를 얻어야 한다.
	- 일반적으로 training error는 모형의 capacity가 증가할
	 수록 최소의 가능한 error 값으로 점근적으로 감소한다.
	- generalization error는 모형 capacity에 대해 U자 모양을
	 그린다.
	- ![](/public/img/머신러닝/img/img_1.png)


- 지금까지는 선형 회귀 같이 parametric한 모형만 봤다. 하지만
 임의의 크기만큼 극단적으로 높은capacity에 도달하기 위해
 **non\-parametric**한 모형을 볼 것이다.
 parametric 모형은 크기가 유한하고 데이터 관찰 전에
 고정되는 파라미터 벡터로 설명되지만 non\-parametric 모형은
 그런 제한이 없다.
	- 어떤 경우에 비모수 모형은 실제로는 구현 불가한
	 이론적인 추상화지만(모든 가능한 확률 분포를 탐색하는
	 알고리즘) 모형의 complexity를 training set size의
	 함수로 만듦으로써 practical한 비모수 모형을 만들 수
	 있다.
	- **nearest neighbor regression**이 그러한
	 알고리즘이다.
		- 최근접 이웃 회귀 모형은 $\mathbf{X}$와
		 $\mathbf{y}$를 training set에서 저장하고 test
		 point $\mathbf{x}$를 분류해달라는 요청을 받으면
		 training set에서 가장 가까운 entry를 찾아서 연관된
		 regression target을 반환한다.
		- 즉, $\hat{y} = y_i$, where $i = \text{arg} \text{
                              min} \parallel \mathbf{X}<em>{i,:} - \mathbf{x} \parallel^{2}</em>{2}$이다.
- 이상적인 모형은 데이터를 생성하는 실제 확률 분포를 아는
 신과 같은 존재지만 그러한 모형도 분포에 존재하는 노이즈로
 인해 에러가 있을 수밖에 없다.
	- 지도학습의 케이스에서 $\mathbf{x}$에서 $y$로의 맵핑은
	 태생적으로 확률적이거나, $y$는 $\mathbf{x}$에 포함되지
	 않은 변수들을 포함한 것들을 변수로 하는
	 deterministic한 함수이다.
	- 실제 분포 $p(\mathbf{x}, y)$을 통해 만든 예측에서 생긴
	 error를 **Bayes error**라고 한다.
- training error와 generalization error는 training set의
 사이즈에 따라 달라질 수 있다.
	- generalization error의 기댓값은 training example의
	 수가 증가할 때 절대 증가할 수 없다.
	- 비모수 모형에서 더 많은 데이터는 best possible error에
	 도달하기 전까지는 항상 더 나은 generalization을
	 가져온다.
	- 최적 capacity 이하인 모든 fixed parametric 모형은
	 Bayes error를 초과하는 error 값에 점근적으로 접근할
	 것이다.
	- 아래 그림에서 볼 수 있듯이 최적 capacity에 도달해도
	 training error와 generalization error 사이에는 큰 갭이
	 있을 수 있다. 그런 경우에는 더 많은 training example을
	 모아서 갭을 줄여나갈 수 있다.





![](/public/img/머신러닝/img/img_2.png)









![](/public/img/머신러닝/img/img_3.png)










## 5\.2\.1 The No Free Lunch Theorem


공짜 점심이 없다는 말은 머신 러닝에서도 적용된다.



 모든 데이터 분포에 대해 전역적으로 우월한 머신러닝 모델을
 만들 수는 없다. 여러 데이터 셋을 통해 만들어진 모형들 모두
 관찰하지 않은 데이터에 대해서는 비슷한 에러 수준을 보여줬다.
 



 다행히도 이런 결과는 모든 데이터 생성 분포에 대해 평균을
 냈을 때만 적용된다. 우리가 실제 세계에서 적용할 때 마주하는
 확률 분포에 대해 몇 가지 가정을 하면 이러한 분포에 대해 좋은
 퍼포먼스를 보여주는 학습 알고리즘을 고안할 수 있다.
 



 즉, 머신 러닝 연구의 목적은 전역적으로 우월한 알고리즘이나
 절대적으로 최적의 학습 알고리즘을 찾는게 아니다. 대신 AI
 에이전트가 경험하는 '실제 세계'에 적절한 분포가
 무엇인지 이해하고, 어떤 종류의 머신 러닝 알고리즘이 우리가
 신경 쓰는 데이터 생성 분포에서 추출된 데이터에 좋은 성능을
 보여주는지 이해하는 것이다.
 


## 5\.2\.2 Regularization


- 지금까지 모델을 수정한 방법은 input featuer 차수를
 조절하거나 함수를 더하거나 빼서 capacity를 조절하는
 것이었다. 하지만 함수의 갯수보다 알고리즘에 큰 영향을
 끼치는 것은 함수 자체의 속성이다.
- 선형 회귀는 non\-linear한 영역에서는 잘 작동하지 않는다.
- 따라서 함수의 갯수 뿐 아니라 함수의 종류도 잘 선택하여
 함수의 hypothesis space를 제어하고, 이를 통해 알고리즘의
 성능을 조절할 수 있다.
- 또한 학습 알고리즘에게 hypothesis space 내의 한 솔루션을
 다른 솔루션보다 선호한다는 사실을 알려줄 수 있다.
- 예를 들어 선형 회귀에 **weight decay**를
 포함하도록 할 수 있다.
 


	- 그러면 최소화 목적식은 다음과 같다
	- $$J(\omega ) = \text{MSE}_{\text{train}} + \lambda
                            \omega^{T}\omega$$
	- $\lambda$가 0이면 아무 영향이 없지만, $\lambda$가
	 커질수록 weight의 크기는 작아져야 한다.
	- $J(\omega)$를 최소화하려면 weight의 크기나 training
	 error를 줄여야 한다.
	- weight decay를 높여서 weight를 작게 할 수록 모형은
	 단순해진다.
	- ![](/public/img/머신러닝/img/img_4.png)
	- 이를 일반화하면 모형의 cost function에
	 **regularizer**를 추가하는 것으로
	 생각할 수 있다. weight decay의 경우 regularizer는
	 $\Omega (\omega) = \omega^{T} \omega$이다.
	- 함수에 대한 선호도를 나타내는 것이 hypothesis
	 space에 멤버를 추가하거나 제외하는 것보다 더
	 일반적인 capacity 조절 방법이다. hypothesis
	 space에서 함수를 제외시키는 것은 모든 다른
	 hypothesis space 내의 함수를 제외된 함수보다
	 무한하게 선호하는 것으로 볼 수 있다.
	- regularization은 보통 training error보다
	 generaliztion error를 줄이려는 목적으로 사용된다.
