---
layout: post
title:  "[논문/리뷰] Self-Hosted Kubernetes: Deploying Docker Containers Locally With Minikube"
date:   2022-09-14 16:19:30 +0900
categories: Paper Review Kubernetes MiniKube 
---

논문을 읽고 그대로 버리면 아깝지 않느냐는 박사과정 분의 조언을 들은 뒤의 생각이다.
논문을 읽은 뒤에 중요하다고 생각된 부분만 해석하여서 정리하려고 한다.

---
**Abstract**

이 페이퍼에서는, Docker의 샘플 컨테이너에 오케스트레이션 툴인 Kubernetes(이하 k8s)를 적용하여 로컬 시스템에 배포하는 것을 목표로 한다.
위 목표의 목적은 설정과 관리가 필요한 Docker의 컨테이너에 그것들의 적용을 보장하여 클라우드나 on-premise에 배포하기 전에 로컬 시스템에서 성공적으로 그것들이 적용되었는지 확인하는 것이다.
on-premise 배포는 보통 파이낸스나 헬스케어 같은 민감한 정보를 다루는 단체에서 사용되는 방식이다.
이러한 단체들은 보안을 이유로 클라우드에 정보를 업로드 하는 것을 꺼려하지만 그들의 어플리케이션에도 여전히 scaling이 요구된다.
* on-premise : 서버를 클라우드 환경이 아닌 자체 설비로 보유하고 운영
* scaling : 많은 수의 사용자 요청을 처리하거나 서버의 부하량을 관리하기 위해 기존 구성(서버, 컴퓨터)을 확장하는 프로세스 [1]

**Introduction**

*A. What is Contanerization*

Containerization은 어플리케이션들(설정 파일, 라이브러리, 종속성을 포함)을 bundling 해주는 과정이다. 어플리케이션을 효율적으로 실행하기 위해 사용된다.
* bundling : 여러 개로 흩어져 있는 파일들을 압축, 난독화 등을 하여 하나의 파일로 모아주는 것 [2]
  
이러한 bundle을 컨테이너라고 하는데, Virtual Machines(이하 VM)들 보다 가벼운, 하나의 대안이다.

Fig. 1. VM과 컨테이너의 차이점
![Figure 1](/assets/images/220914/Figure1.png)

*B. What is a Container*

컨테이너의 정의에 관해서는 상술 하였기 때문에 넘어가려 한다.

컨테이너들은 호스트 시스템과 다른 컨테이너들과는 독립적으로 동작한다.
컨테이너는 VM과 비슷한 환경을 제공해주지만, VM 처럼 모든 HW와 OS를 시뮬레이팅 하지는 않는다.
여러개의 다른 컨테이너들은 모두 호스트 시스템의 OS를 공유하여 사용한다.
컨테이너들은 모두 분리된 프로세스를 사용하기 때문에 가벼움을 제공한다.

아래 사진은 컨테이너의 구조이다.

![Picture 1](/assets/images/220914/Pic1.png)




***Reference***

1. Self-Hosted Kubernetes: Deploying Docker Containers Locally With Minikube, Ruchika Muddinagiri et al., 2019 International Conference on Innovative Trends and Advances in Engineering and Technology (ICITAET), [https://ieeexplore.ieee.org/document/9170208](https://ieeexplore.ieee.org/document/9170208)

[1] https://www.geeksforgeeks.org/overview-of-scaling-vertical-and-horizontal-scaling/
[2] https://tech.weperson.com/wedev/frontend/bundling-transpiler/