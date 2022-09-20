---
layout: post
title:  "[논문/리뷰] Self-Hosted Kubernetes: Deploying Docker Containers Locally With MiniKube"
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
등을 기반으로 하는 다른 도구와 차별점을 갖는다.

**II. The Theory of Kubernetes**

*A. What is Kubernetes*

k8s란, 여러 서버에 배포되어있는 컨테이너들을 조율(스캐쥴링 및 매니지를 의미)하는 도구다. 
k8s는 Linux 컨테이너에 관한 작업을 자동화하여, manual processes와 관련된 것을 제거한다.

*B. Terms in Kubernetes*

이는 영어로 보는 것이 보다 직관적인 이해에 도움을 줄 것 같아 영어 원문을 서술한다.
1. Node : It is a worker machine
2. Cluster : A group of nodes
3. Pods : They are the atomic units which can contain one or more containers.
4. Deployments : They provide the most common way to create and manage pods.
5. Service : A service declares how pods can be accessed. It is the virtual server inside the node. When pods need to communicate, they use 'services'.

*C. Ways to use Kubernetes*

1. Hosted : Hosted k8s는 k8s를 사용하는 클라우드 서비스들을 말한다. 사용자가 k8s를 사용하고 싶으면, 클라우드 서비스 프로바이더에게 요청만 하면 된다. 이는 머신들을 확장하고, 뛰어난 가용성을 제공하기 위한 쉬운 선택 옵션 중 하나다.

2. Self-Hosted : Self-Hosted k8s는 단일 서버에서 k8s를 이용하여 어플리케이션을 테스트 혹은 배포할 수 있게 해준다.

이 논문(paper)에선 로컬에서의 self-deployed solutions를 구축하는데에 집중한다.

*D. How to work with MiniKube*

논문의 필자는 윈도우 10에서 Docker Toolbox, kubectl, MiniKube + Virtual Box를 활용하여 진행하였다.  

MiniKube를 사용한 이유 : 윈도우 환경에서, MiniKube를 이용하면 Virtual Box와 Hyper-V를 포함한 여러 드라이버를 사용할 수 있기 때문에.  

Docker Toolbox를 사용 한 이유 : Minukube는 이미 Docker daemon을 깔며 사전에 설치되어 있다. 하지만 Docker client와 통신을 하기 위해선 Docker Toolbox가 필요하다.

Virtual Box를 사용 한 이유 : MiniKube는 Virtual Box를 이용하여 노드(Virtual machine)을 생성한다. 이러한 Virtual machine을 boot2docker이라고 하는데, 이는 2GB의 메모리와 2 코어의 CPU만을 요구하기 때문에 적은 리소스로 단일 노드를 생성하는데에 적합하다.  

MiniKube는 단일 노드 틀러스터만을 지원하지만, 개발 용도에서는 단일 노드의 클러스터면 충분하다.  

**III. Practical Implementation**

*A. Flowchart of implementation*
  
![Figure 4](/assets/images/220914/Figure4.png)

Fig 4. Flowchart of implementation, github.com/superwonso 재구성

*B. Methodology of Implementation*

파이썬을 이용한 k8s 서버 구축 방법에 대해서는 추후 서술 예정.

**IV. Conclusion and Applications**

MiniKube는 kubectl을 이용한 뛰어난 CLI 에서의 작업을 제공한다. Docker daemon에 내장되어 있는 MiniKube는 이를 이용하여 배포할 때 유용함을 제공한다. 또한 워크로드를 관리하기 위하여 온-클릭 확장 옵션을 제공한다. MiniKube를 이용한 어플리케이션 컨테이너가 성공적으로 작동하는지 확인 한 후에는 IT 기업은 클라우드 상품을 이용하여 보다 쉽게 배포할 수 있을 것이다. 이는 k8s를 이용하여 보다 효율적인 컨테이너 구성과 관리를 제공해줄 것이다.  
현재 MiniKube와 도커 컨테이너를 사용하는 어플리케이션은 많이 있다. 전에도 언급했듯이, 경제나 건강같이 고객의 민감한 정보를 다루는 domain들은 클라우드에 업로드 하기에는 보안 리스크가 크다. Minikube를 이용한다면 이러한 환경에서도 보다 쉽게 조직을 확장 할 수 있을 것이다.  
또한 클라우드와 같은 곳에 실제로 배포하기 전에 로컬 테스트 환경을 구성할 때에도 유용함을 제공할 것이다. 

---

***Reference***

Original : Self-Hosted Kubernetes: Deploying Docker Containers Locally With MiniKube, 
Ruchika Muddinagiri et al., 2019 International Conference on Innovative Trends and Advances in Engineering and Technology (ICITAET), [https://ieeexplore.ieee.org/document/9170208](https://ieeexplore.ieee.org/document/9170208)  

[1] [https://www.geeksforgeeks.org/overview-of-scaling-vertical-and-horizontal-scaling/](https://www.geeksforgeeks.org/overview-of-scaling-vertical-and-horizontal-scaling/)  
[2] [https://tech.weperson.com/wedev/frontend/bundling-transpiler/](https://tech.weperson.com/wedev/frontend/bundling-transpiler/)  
