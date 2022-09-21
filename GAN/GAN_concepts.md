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

## Adversarial nets
![수식](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Lzv9WQqVErrkv4TUmw2%2Fuploads%2FHaQuzfKjH9FwPGzys4Om%2Ffile.png?alt=media)

`>` 첫번째 항은 real data를 D에 넣었을 때의 값을 log를 취했을 때의 기댓값이다.

`>` G(z)는 fake data z를 사용해 G가 data distribution을 모사하여 값을 만든다. 두번째 항의 1-D(G(z))를 log를 취했을 때의 기댓값이다

