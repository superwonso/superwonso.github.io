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

**I. Introduction**

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
컨테이너는 어플리케이션과 어플리케이션을 지원하기 위한 파일들(의존성, dependencies)을 포함하고 이를 호스트 OS에서 돌린다.

*C. Insight into the working of Docker*

Docker은 개발자들을 위한 플랫폼으로, 컨테이너 위에서의 개발, 배포, 어플리케이션 실행을 위해 쓰인다.

1) 컨테이너는 이미지를 돌리며 실행된다. 이미지는 실행 가능한 패키지들과 필요한 모든 것들(코드, 런타임, 라이브러리, 환경 변수, 설정 파일)을 포함한다. Docker에서는 이러한 이미지들을 DockerFile이라고 칭한다. 베이스 이미지에는 항상 DockerFile이 구축된다.
DockerHub은 여러 도커 이미지들의 Public Repository로, Public/Private 여부는 설정하여 Repository를 생성할 수 있다.
2) 가상화 된 디스크와 네트워크 인터페이스 등에 접근하고, 정보를 DockerFile에 제공한다.
3) "docker build" 커맨드를 이용하여 DockerFile 또는 이미지를 기반으로 컨테이너를 빌드한다. 
4) DockerFile에 정의되어 있는 어플리케이션에 대한 빌드는 어디서 돌아가던지 동일하다. Docker 클라이언트는 콘테이너를 생성하기 위해 Docker daemon와 통신한다.
모든 이미지들은 Public 또는 Private Registries에 저장된다.

*D. What are Container Orchestration Tools?*

Docker가 가장 많이 쓰이는 컨테이너 도구이다. 보통 하나의 어플리케이션에는 여러 개의 컨테이너가 같이 동작한다. 따라서 이러한 컨테이너들을 관리해줄 도구가 필요하다.
컨테이너 조율(Orchestration) 도구들이 이러한 역할을 해 준다.
컨테이너 조율 도구는 크고 역동적인(dynamic) 환경이라 하더라도 컨테이너들의 수명 주기(life-cycle)를 관리해줄 수 있다.
현재 존재하는 컨테이너 조율 도구에는 Kubernetes, Docker Swarm, Apache Mesos 등이 있다.
컨테이너는 일반적으로 복제된 그룹에 있는 호스트에 배포된다.
컨테이너 조율 도구는 deployment를 스케쥴링하고, 사전에 정의된 제약 사항들(CPU, 가용 메모리 등..)을 기반으로 컨테이너를 배치하는 데 가장 적합한 호스트를 찾는다.
컨테이너가 호스트에서 돌아가는 동안, 컨테이너 조율 도구는 DockerFile와 같은 컨테이너 정의 파일을 기반으로 컨테이너의 생명 주기를 관리한다.

on-premises 서버부터 클라우드 인스턴스까지, 어떤 환경이던간에 컨테이너만 돌아간다면  컨테이너 조율 도구를 사용할 수 있다.

*E. Why to choose Kubernetes for a containerization tool?*

컨테이너 조율 도구는 각자 장단점이 있지만 k8s는 아키텍쳐, HA needs, 유연성, 러닝 커브
등을 깁나으로 하는 다른 도구와 차별점을 갖는다.

**II. The Theory of Kubernetes**

*A. What is Kubernetes*

*B. Terms in Kubernetes*

*C. Ways to use Kubernetes*

*D. How to work with MiniKube*

**III. Practical Implementation**

*A. Flowchart of implementation*

*B. Methodology of Implementation*

**IV. Conclusion and Applications**

---

***Reference***

Self-Hosted Kubernetes: Deploying Docker Containers Locally With Minikube, 
Ruchika Muddinagiri et al., 2019 International Conference on Innovative Trends and Advances in Engineering and Technology (ICITAET), [https://ieeexplore.ieee.org/document/9170208](https://ieeexplore.ieee.org/document/9170208)

[1] https://www.geeksforgeeks.org/overview-of-scaling-vertical-and-horizontal-scaling/
[2] https://tech.weperson.com/wedev/frontend/bundling-transpiler/