---
layout:     post
title:      "Normalizing Flow - RealNVP 이론 과 구현까지 "
date:       2021-5-06 00:00:00
author:     "MJ Shin"
tags:
    - Machine Learning
    - Normalizing Flow
    - ML
use_math: true
comments: true
---
## Normalizing Flow 개념 

<p> 최근 Kubernetes 공부를 시작으로 서버 구성 작업을 하던 중 회사 프로젝트에서 머신러닝 모델의 퍼포먼스 증가를 위해서 normalizing flow를 접목시키게 되었습니다. 단순하게 normalizing flow 모델을 그대로 적용하는 것은 아니지만 이 과정을 위해서 공부하고 기본적인 구현까지 한 내용들을 정리하고자 블로그를 씁니다. 

<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure2.PNG?raw=true">

<p> Normalizing Flow의 컨셉은 단순하게 말하면 "_simple distribution_을 _complex distribution_으로 변환한다!" 입니다. 이때, 논문에서는 simple distribution에 연속적으로 _invertible한 mapping_을 사용해서 complex distribution으로 변환한다고 설명하고 있으며 여기서 중요한 점은 _invertible mapping_을 적용한다는 점입니다. 위의 그림은 Normalizing Flow를 가장 잘 설명하고 있는 그림입니다. 

<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure1.PNG?raw=true">

<p> Normalizing Flow에서 "z" 를 mapping하는 함수 f는 1-1 mapping function 이어야 하며 이 점이 매우 중요한 포인트입니다. 

<p> 위의 사진은 논문에 있는 figure 입니다. Inference 시에는 Data space X의 격자무늬가 latent space Z로 매핑되면서 격자가 왜곡이 생기는데 이때 왜곡된 줄들이 엉켜있지 않습니다. 이는 topological property 측면에서 Data space X와 latent space Z가 homeomorphism(위상동형사상)이라는 것을 의미합니다. 반대로 Generation 시에는 latent space Z의 격자무늬가 Data space X로 매핑되면서 격자의 왜곡이 생기며 이때도 역시 격자가 서로 꼬이는 것이 없습니다. 즉, 이 mapping이 1-1 mapping이며 공간의 위상적 성질을 보존하는 위상동형사상 f  라는 것을 보여주는 그림이빈다. 

> 논문이나 다른 블로그들에서는 위의 그림에서 중요한 부분에 대해서는 설명이 되어있지 않았습니다. 격자무늬의 distortion에 대해서 아래 제가 reference로 단 Postech AMI lab의 유투브 영상에서 교수님(?)께서 이에 대한 설명을 해주시는게 들려서 이런 것들도 다 의미가 있었구나 하였던 그림입니다. 

<p> 그래도 나름 이론적인 부분과 구현을 모두 다루고자 하기 떄문에 이런 포인트는 다 같이 생각해두고 넘어가면 좋을 것 같아서 남깁니다 ㅎㅎ

<p> 다시 본 내용으로 돌아가면, Normalizing Flow의 연속적인 mapping을 통한 distribution의 변환이 가능한 이유는 **Change of Variables Theorem** 을 통해서 가능해집니다. 

<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure3.PNG?raw=true">

<p> 그리고 논문에서 처음 등장하는 수식이 위의 수식입니다. 위 수식이 표현하고자 하는 바는 이렇습니다. random variable "z" 가 distribution $$z\sim\pi(z)$$ 을 따른다고 할 때, 1-1 invertible mapping function "f" 를 통해서 새로운 random variable "x" ($$x=f(z)$$)를 구성할 수 있으며, 이때 "x"의 probability distribution $$p(x)$$를 구하는 방법은 수식과 같다는 것입니다. 

<p> 이 부분에 대해서 깊게 이해 할 필요는 없지만 자세히 조금 더 자세히 살펴보고자 합니다. 해당 내용은 논문의 refernece "Information-Maximization Approach to Blind Separation and Blind Deconvolution (Bell, A. J., & Sejnowski, T. J.) (1995)" 에서 자세히 다뤄지고 있습니다. 논문 자체의 주제는 Mutual Information 에 관한 내용이지만 우리에게 필요한 부분만 가져와서 이해해 보도록 하겠습니다. 

<p> Mutual Information을 모르시면 우선 수식 $$I\(Y, X\) = H\(Y\) - H\(Y\barX\)$$ 라는게 있다고 생각하시면 될 것 같습니다. 그리고 논문에서는 이렇게 말합니다. When we pass a single input x through a transforming function g(x) to give an output variable y, both $$I\(y, x\)$$ and $$H\(y\)$$ are maximized when we align high density parts of the probability density function (pdf) of x with highly sloping parts of the function g(x).  

<p> 즉, random variable x 가 1-1 mapping function "g"를 통해서 새로운 random variable "y"를 구성할때 ($$y=g(x)$$), 위의 MI 수식의 $$I\(y, x\)$$ 과 $$H\(y\)$$ 는 "x"의 확률 밀도 함수 (pdf)의 가장 높은 확률이 부여되는 곳 (고밀도 부분(?), high density parts) 을 1-1 mapping function "g"의 가장 큰 slope가 구성되는 곳과 매핑이 되는 경우 두 값이 최대가 된다라는 설명입니다. 자세한 설명은 논문에 나와있으니 살펴보시면 될 것 같습니다 ㅎㅎ 지금 이 글에서 중요한건 여기가 아니니까요.

<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure4.PNG?raw=true">

<p> 그래서, the pdf of the output, $$p(y)$$, can be written as a function of the pdf of the input $$p(x)$$. 라고하며 위의 수식을 설명합니다. 이 수식이 매우 익숙하실텐데 이게 normalizing flow 논문의 (1)번 수식입니다. 정확히는 위 reference 논문의 (2.12)를 보시면 더 같다는 것을 아실 수 있습니다. 

<img src="https://github.com/170928/170928.github.io/blob/master/_images/normalizing_flow/figure5.PNG?raw=true">

#### Reference 
>1. https://www.youtube.com/watch?v=hwpNjszbHBM (Postech AMI lab 동영상입니다. 중간중간 들리는 교수님의 말씀에서도 큰 도움을 받았습니다. 이런 연구실에서 박사를 하면 엄청 즐겁게 배울게 많겠네요 ㅎㅎ )
>2. http://bigdata.dongguk.ac.kr/lectures/med_stat/_book/%ED%99%95%EB%A5%A0%EB%B3%80%EC%88%98%EB%B6%84%ED%8F%AC.html (오래전에 공부해서 잊었던 통계학적인 설명에 대해서는 이 reference에서 많이 공부했습니다.)