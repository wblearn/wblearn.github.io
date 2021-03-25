---
layout:     post
title:      "Kubeflow 튜토리얼 (1)"
date:       2021-3-25 00:00:00
author:     "MJ Shin"
tags:
    - Kubeflow
    - Kubernetes
    - ML
    - MLOps
---
## Kubeflow 개념 

<p> 이 블로그는 kubeflow 를 공부하고 기록을 남기기 위한 글 입니다. 처음 공부하고 일과 연구에 적용시키기 위해서 기록하는 글이라서 틀린 부분이나 오류가 있을 수 있습니다. 언제든지 잘못된 부분이나 새로운 의견이 있으시면 알려주세요 :)

<p> Kubeflow는 기본적으로 MLOps를 굉장히 편하게 해주는 Kubernetes 환경에서 작동하는 시스템입니다. 그렇기 때문에 Kubernetes 에대한 기본적인 지식도 필요합니다. 추후 다른 글에서 Kubernetes 튜토리얼도 다룰 생각입니다.

<p> Kubeflow는 MLOps 의 모든 lifecycle을 쉽고 간편하게 해준다고 공식 유튜브에서도 소개하고 있습니다. (https://www.youtube.com/watch?v=SRq21GRT31g)

<p> Kubeflow를 사용하는 것의 장점은 사진 한장으로 이해할 수 있습니다. 

<img src="https://github.com/170928/170928.github.io/blob/master/_images/ml_operation_overview.png?raw=true">

<p> 위의 이미지는 machine learning을 사용하여 service를 하고자 할 때 실제로 필요로 되어지는 사항들을 표기한 것입니다. 생각보다 Machine learning code가 갖는 비중은 엄청 작고 다른 부가적인 요소들이 대부분의 비중을 차지하고 있는 것을 알 수 있습니다. 

<p> 그리고, 이런 요소들은 Kubernetes와 Kubeflow가 없다면 각기 다른 환경에서 작동되는 상황을 가정하였을때 현실적으로 모두를 제대로 관리할 수 없게 됩니다...

<p> 실제로 ML을 처음 공부시작하면서 집중하게 되는 것은 모델을 구현하고 그에 따라서 논문에 나오는 여러가지 metric에 따라 모델의 성능을 향상시키는 것에 집중하게 됩니다. 

<p> 그러나, ML을 단순히 연구의 대상이 아니라 실제로 product를 만들고 이를 위해 ML을 사용하기 위해서는 ML pipeline이 훨씬 더 중요하게 됩니다.

<p> ML의 pipeline은 다음과 같이 크게 정의할 수 있습니다. 

1. load data : 사용하고자 하는 데이터를 db 와 같은 곳에서 가져옵니다. 
2. data analysis & feature engineering : 데이터를 분석하고 ML 모델에 적용하기에 적합하도록 feature를 조절합니다. (개인적으로 이 단계가 제일 중요하다고 생각합니다)
3.  model implementation & validation : 위에서 전처리된 데이터를 다루기 위한 ML 모델을 구현하고 학습을 수행하며, 모델에 대한 검증을 진행합니다.
4.  model serving : 학습된 모델을 사용해서 service 를 제공할 수 있도록 구성합니다. 



## Kubeflow 설치

<p> 간단한 개념에 대해서 살펴보았으니, 바로 Kubeflow를 어떻게 설치하는지 부터 살펴보겠습니다. 실제로 해보고 나서 개념을 다시 살펴보면 훨씬 잘 이해가 된다고 생각합니다.

<p> 



#### Reference 
>1. 가장 큰 도움을 받은 블로그 입니다! http://ghcksdk.com/kubeflow-installation/ 
>2. https://lsjsj92.tistory.com/579 
>3. https://www.youtube.com/watch?v=sRQECN7LsbI
