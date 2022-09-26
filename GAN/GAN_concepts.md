# Generative Adversarial Nets

## Abstract

`>` Genrative model(G)은 데이터 분포를 capture 한다.

`>` Discriminative model(D)은 sample data가 G로 부터 만들어진 데이터가 아닌 training data에서 나왔을 확률을 추정한다.

* G의 훈련 과정은 D가 실수를 할 확률을 최대화하도록 하는 것이다. (여기서 실수란, G에서 만들어진 데이터가 training data에서 나왔을 거라고 착각(?)하게 만드는 것)
* G가 데이터 분포를 recovering하게 되면 D가 제대로 판별할 확률은 1/2이 된다.
* G와 D가 다층 퍼셉트론으로 정의된 경우, 모든 시스템은 역전파 기법(backpropagation)으로 훈련된다.

## Introduction

`>` 딥러닝에서 성공?한 모델은 대부분 dropout과 backpropagation에 알고리즘 기반이다.

`>` 기존의 Deep generative 모델들은 다루기 힘든(intractable) 확률 연산에 의해 임팩트 있지 않았다.

`>` D는 sample이 training data 분포인지 model 분포인지 구분하여 결정해야 한다. 

`>` *D는 위조화폐를 검출하려는 경찰이라고 생각하면 된다. 반대로 G는 진짜같이 보이는 위조 화폐를 만들어 D를 속이려는 위조지폐범이다. 이 둘은 경쟁을 통해 완벽한 검출을 하려는 경찰(D), 완벽한 위조지폐를 만드려는 위조지폐범(G)이 되고 결국 경찰이 구별할 수 있는 확률이 50%가 되면 경찰은 더이상 진짜와 가짜를 구분할 수 없는 상태에 도달한다*



### Abstract에서 말했듯이...  
> `-`Genrative model은 다층 퍼셉트론을 통해 랜덤 노이즈를 전달하여 샘플을 생성한다.  
`-` Discriminative model 또한 다층 퍼셉트론이다.  
`-` GAN은 dropout과 backpropagation 알고리즘을 기반으로 위 두개의 모델을 학습시킨다.  
`-` G를 통해 생성된 sample은 오로지 fowardpropagation을 통해 학습된다.  

## Related work
이거는 패스..

## Adversarial nets
![수식](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Lzv9WQqVErrkv4TUmw2%2Fuploads%2FHaQuzfKjH9FwPGzys4Om%2Ffile.png?alt=media)

`>` 첫번째 항은 real data를 D에 넣었을 때의 값을 log를 취했을 때의 기댓값이다.

`>` G(z)는 fake data z를 사용해 G가 data distribution을 모사하여 값을 만든다. 두번째 항의 1-D(G(z))를 log를 취했을 때의 기댓값이다.

`>` D의 레이블(G를 통해 만들어진 데이터와 train data)을 올바르게 지정할 확률을 최대화하기 위해 훈련을 시킴  
`+` D(x)는 x가 real data라고 생각한다면 1을 출력하고, G가 생성한 fake data라고 생각하면 0을 출력한다.

`>` G는 $log(1-D(G(z)))$의 값을 최소화 하기 위해 훈련을 시킴  
`+` 만약 G가 real data처럼 보이는 fake data를 만들었다면 D는 이를 real data라고 생각하여 1을 출력할 것이다. 그렇다면 $log(1-D(G(z)))$는 $log(0)$이 되어 마이너스 무한대가 된다.

`>` 정리하자면 G의 성능이 좋다면, $V(D,G)$가 마이너스 무한대, 즉 최소화되려고 하고,  D의 성능이 좋다면, $V(D,G)$가 0이 되려고한다. 즉 $V(D,G)$를 최대화시키려 한다. 이를 해당 논문에선 two-player minmax game이라고 한다.

**Figure 1)**
![분포](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Lzv9WQqVErrkv4TUmw2%2Fuploads%2F39NU70gaap9JBh5zqwX8%2Ffile.png?alt=media)
>    파란선: discriminative distribution `->` D (판별모델)   
     검정선: data generating distribution ; $p(x)$  `->` real data  
     초록선: generating distribution ; $p(g)$ `->` fake data `->` G가 만들어낸 data

`>` 검정선과 초록선이 점점 일치해가는 모습을 보이는데 이는 G가 점점 real data와 유사한 fake data를 만들어내고 있음을 의미한다.

`>` 파란선이 점점 흔들리지(?)않는 것을 확인할 수 있음. 판별을 안정적으로 한다고 해석

## Theoretical Results

수식은 좀더 봐야할것 같습니다...

## Experiments

`>` generator nets은 ReLU(rectifier linear activations)와 sigmoid를 혼합하여 사용하였다. 이론적인 framework에서는 G의 중간층에서 dropout과 noise를 허용하지만 논문에서는 noise를 generator nets의 가장 밑에서 noise를 입력하였다.

`>` Discriminator nets은 maxout을 사용하였다. 또한 D를 훈련시킬때 Drop out이 적용되었다.

`>` G에서 생성된 데이터$p(g)$를 Gaussiam Parzen window에 피팅...  
`+` likelihood function(우도함수)는 주어진 관측값으로부터 데이터를 잘 표현하는 모수를 얻는다고 이해하면 됨 (우도함수가 값이 가장 클때의 모수가 데이터를 잘 표현한다. 이 모수, 즉 파라미터를 $theta$)
