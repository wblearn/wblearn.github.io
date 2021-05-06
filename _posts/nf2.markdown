---
layout:     post
title:      "Normalizing Flow - RealNVP 이론 과 구현까지 (2)"
date:       2021-5-06 00:00:00
author:     "MJ Shin"
tags:
    - Machine Learning
    - Normalizing Flow
    - ML
use_math: true
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---
## Normalizing Flow -> RealNVP Implementation & Theory


<p>
이제 본격적으로 realNVP 논문의 내용을 살펴보도록 하겠습니다. 이론적인 부분보다 이번 포스팅에서는 간단한 realNVP 개념과 구현사항을 살펴보겠습니다.

realNVP 논문에서는 위의 1-1 invertible mapping function을 neural network로 구성하였으며, affine coupling layer 라고 정의하고 있습니다.

이 논문에서는 이 affine coupling layer가 시작이자 끝이라고 할 수 있습니다. 

<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure8.PNG?raw=true">

normalizing flow에서의 중요한 점은 1-1 invertible mapping function을 Jacobian을 계산하기 효율적이면서 complex distribution을 잘 만들어 낼 수 있는 function을 만들어 내는 것입니다. 그리고 그 방법으로 "input 의 일부는 invert 하기 쉬운 방법으로 update되고 나머지는 complex way로 update 한다" 라고 논문에서는 언급합니다. 쉽게 설명하자면, 이전 포스팅에서의 z1 = f(z) 와 같은 과정에서 z가 5차원 이라고 가정하면 1, 2, 3 차원 까지는 쉬운 방법으로, 4, 5차원은 복잡한 방법으로 업데이트 하는 것입니다. 


<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure9.PNG?raw=true">

그리고, 이러한 방법의 장점은 normalizing flow의 loss function을 계산하는 과정에서 (이전 포스팅 참조), Jacobian의 계산이 매우 효율적으로 이루어 질 수 있다는 것입니다. 위의 수식은 realNVP에서 말하는 affine coupling layer를 사용하였을 때 loss function이 되는 Jacobian Matrix이며 위의 행렬을 Jacobian을 계산하게 되면 아래와 같아짐을 알 수 있습니다.
$$exp[\sum_{j} s(x_{1:d})_j]$$

<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure10.PNG?raw=true">

더해서, invertibility 측면에서 s 와 t를 적용하는 inference 과정과 유사하게 계산할 수 있다는 점이 큰 장점으로 제시하고 있습니다. 하지만 실제로 위의 Jacobian 과정에서 필요하지 않아서 그다지 의미는 없어보이네요 ㅎㅎ 

논문에서는 s 와 t 함수 또한 neural network로 구현하고 있으며, deep convolutional network 를 적용하고 있습니다. 자세한 내용은 구현을 살펴볼 때 보겠습니다. 
</p>

<p>
<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure11.PNG?raw=true">

친절하게도 이 논문에서는 위 수식 (7) 번의 실제 구현시에 방법을 설명해주고 있습니다. input dimension의 1:d 와 d+1:D 를 masking 방법을 통해서 구현하였다는 것을 알려줍니다. 


<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure12.png?raw=true">

최종적으로 realNVP를 사용한 모델 학습의 경우 위의 목적식을 가지고 학습을 수행하게 됩니다. 
</p>

