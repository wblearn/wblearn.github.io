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

<p> 이 블로그는 kubeflow 를 공부하고 기록을 남기기 위한 글 입니다. 처음 공부하고 일과 연구에 적용시키기 위해서 기록하는 글이라서 틀린 부분이나 오류가 있을 수 있습니다. 언제든지 잘못된 부분이나 새로운 의견이 있으시면 알려주세요 :)

<p> Kubeflow는 기본적으로 MLOps를 굉장히 편하게 해주는 Kubernetes 환경에서 작동하는 시스템입니다. 그렇기 때문에 Kubernetes 에대한 기본적인 지식도 필요합니다. 추후 다른 글에서 Kubernetes 튜토리얼도 다룰 생각입니다.

<p> Kubeflow는 MLOps 의 모든 lifecycle을 쉽고 간편하게 해준다고 공식 유튜브에서도 소개하고 있습니다. (https://www.youtube.com/watch?v=SRq21GRT31g)

<p> 우선, Kubeflow를 사용하는 것의 장점에 대해서 살펴보겠습니다. 

<img src="https://github.com/170928/170928.github.io/blob/master/_images/ml_operation_overview.png?raw=true">

<p> 위의 이미지는 machine learning을 사용하여 service를 하고자 할 때 실제로 필요로 되어지는 사항들을 표기한 것입니다. 생각보다 Machine learning code가 갖는 비중은 엄청 작고 다른 부가적인 요소들이 대부분의 비중을 차지하고 있는 것을 알 수 있습니다. 

<p> 그리고, 이런 요소들은 Kubernetes와 Kubeflow가 없다면 각기 다른 환경에서 작동되는 상황을 가정하였을때 현실적으로 모두를 제대로 관리할 수 없게 됩니다...

<p> 이러한 어려움을 Kubeflow는 한번에 해결하여 MLOps를 매우 편리하게 해줍니다.

