---
layout: post
title:  "[논문/리뷰]Blockchain for AI: Review and Open Research Challenges"
date:   2022-07-04 18:10:00 +0900
categories: Paper Review Blockchain AI Machine%20Learning IEEE%20Access
---

평소 블록체인 기술에 관하여 관심을 갖고 있었다.

이에 관하여 교수님께 여쭤보니 블록체인 자체 만으로 무엇을 하는 것 보다는 이를 이용한 연구를 수행하는 것이 좋지 않겠느냐는 답변을 받고, 어떻게 블록체인을 활용할 수 있을까에 관해 찾아보다가 IEEE Access에서 AI에 활용할 수 있지 않을까에 관한 논문을 찾아서 나름대로 정리한 것을 서술해보려 한다.

이 논문에서 AI에 블록체인을 어떻게 활용할 수 있는지에 관해 알아보려 한다.

---

**I. INTRODUCTION**

필자에 따르면, 블록체인은 과장되어 있는 기술(one of the most hyped innovations) 중에 하나라고 한다.  2008년 이래로(Bitcoin을 말하는듯 함) 블록체인은 결제의 자동화, 거래의 추적과 그 방식 등에 관한 혁명적인 기술(disruptive innovation)이었다고 한다. 매우 높은 비율 효율성을 가진 이 기술은 변경이 불가능하고 동기화가 되는 타임스탬프를 기록하여 보안을 높였다고 한다.

이는 암호화폐의 측면에서 서술된 문장인 듯 하고, 필자는 다른 분야에서도(Another promient field) 큰 관심을 받고 있다고 한다. 이 분야는 기계가 수집한 데이터를 기반으로 학습 및 추론을 하고 이를 적용하여 인지 기능을 갖도록 하는 AI 분야다. 2019년 시점에서의 시장 조사에 따르면 2030년까지 AI 분야 시장은 13조 달러 규모까지 성장할 것이라고 한다.

이러한 큰 잠재성을 가진 AI에 블록체인 기술을 어떻게 활용할 수 있을까?

블록체인의 탈중앙화(Decentralized) 특징을 이용하여 Decentralized AI에 활용할 수 있다고한다.

Decentralized AI가 무엇일까?

Decentralized AI는 AI와 블록체인 기술이 융합된 AI 기술로 과정과 적용에 분산화 되어있고 탈중앙화적인 블록체인의 전자적 서명(digitally signed)을 이용하여 신뢰성을 높이고, 보안이 뛰어난 상태의 데이터 공유가 가능하게 해주는 AI의 기술 중 하나이다.

블록체인의 스마트 계약(smart contracts)를 사용하여 데이터의 접근, 생성 등에 관하여 제약을 할 수 있다. 

전자 계약을 기반으로 한 자동화 된 시스템은 AI에서의 학습 및 추론과 적용을 블록체인에서의 노드 채굴자들에 의해 검증되어, 신뢰할 수 있는 결정을 내릴 수 있게 해줄 것이라 한다.

이 논문이 기여하는 바를 아래 다섯가지로 요약할 수 있다고 한다.

1. 블록체인의 주요한 특성 및 기술에 관한 기초와 이를 어떻게 AI에 적용할지
2. AI와 블록체인의 융합이 새로운 탈중앙화 경제의 새로운 생태계에 어떻게 도움을 줄지
3. 기존의 Decentralized AI에 더하여 블록체인 플랫폼, 아키텍쳐, 인프라 유형 그리고 합의 프로토콜에 관한 자세한 분류
4. 여러 분야에서의 블록체인을 활용한 AI가 어떻게 사용되고 있는지에 관한 보고와 논의
5. 미래의 AI Apps에 블록체인의 적용과 확대에 관한 Open Research Challenges 개요

이 논문의 뒷부분은 다음과 같이 구성된다. 

섹션 2(Background)에서는 블록체인과 AI 기술의 배경에 관해서와 기존의 AI 기술을 변환하는 데 어떻게 도움을 주는지에 관하여 논의한다.

섹션 3(Taxonomy)에서는 세부적인 분류를 제시한다.

섹션 4(Blockchain-Enabled AI Applications)에서는 AI를 위한 블록체인 어플리케이션을 설명한다.

섹션 5(Open Research Challenges)에서는 다양한 공개 연구 과제와 문제를 제시한다.

섹션 6(Conclusion)에서는 논문을 마무리한다.

---

**II. Background**

섹션 2에서는 블록체인과 AI에 대한 개요, 블록체인 기술을 활용한 AI와 해당 App을 여러가지 방면으로 전환하는 방법에 대해 논한다.

**A. Blockchain**

블록체인은 오픈 소스이고, 데이터들은 네트워크로 연결된 피어들간에 분산되어 있으며 그 데이터는 불변한다.  블록체인은 블록들의 체인으로, 이들로 렛저(ledger)을 형성한다.

렛저(ledger)는 분산 블록체인 네트워크에 접근하는 참여자들 간에 발생한 거래와 상호작용에 대해 영구적인 기록을 갖고있는다.

비트코인(Bitcoin)을 예시로 들어, 전자 화폐의 유효성을 확인하는 대신에 블록체인 마이닝 노드는 블록에 데이터를 실행, 확인 및 저장을 한다고 설명한다.

스마트 계약(smart contracts)은 사전에 정의되어있는 조건들이 수행되는지를 확인할 수 있는 코드이다. 

이더리움(Ethereum)이 대표적으로 이를 활용하는 전자 화폐인데, 스마트 계약은 트랜잭션(tran-saction)을 이더리움 지갑의 주소에 위탁하고, 해당 트랜잭션에 위탁된 input에 따라서 스마트 계약의 내용이 수행된다.

이더리움은 이더(Ether)라는 자산을 사용하는데, 이는 이더리움 블록체인에서 트랜잭션을 위한 거래 수단으로 사용된다. 이더리움 네트워크의 각각의 참여자들은 유니크한 주소값인 이더리움 주소(Eth-ereum Address, EA)를 갖는다.

일반적으로 블록체인들은 큰 양의 데이터들을 저장하기 위해 비싼 값을 치룬다. 

예를들어, 비트코인 블록체인에서는 1MB당 1블럭을 사용하기 때문에 많은 비용을 치루어야 한다.

이러한 문제를 해결하기 위해, 탈중앙화 되어있는 저장공간을 각각의 데이터와 해시들을 저장하기 위해 스마트 계약을 이용하여 블록들을 연결하여 사용한다.

탈중앙화 되어있는 저장공간 기술에는 Interplanetary File System(IPFS), Swarm, Filecoin, BigChainDB, Storj 등이 있다.

그 중에서도 IPFS에 대해 설명하면, IPFS는 P2P 네트워크로써 분산화 되어 있고 탈중앙화 되어 있는, 컴퓨터들의 노드를 연결하여 일반적인 파일 시스템을 공유하는 기능을 한다. 

이 파일 시스템은 노드 목록을 갖고 있으므로, 파일의 변조를 허용하지 않기때문에 블록체인 네트워크와 유사하게 작동한다고 할 수 있다. 또한 분산되어 저장된다는 특성을 갖고 있으므로, 장치 하나의 연결이 끊어져도 파일에 접근 할 수 있다.

이 기술을 이용하는 대표적인 시스템에는 토렌트(Torrent)가 있다.

**B. Artificial Intelligence (AI)**

Table 1. Latest trends in AI applications and benefits of using blockchain.

![Table_1](2022-07-04-Blockchain%20for%20AI%20Review%20and%20Open%20Research%20Challenge/Table_1.png)

Figure 1. An overview of AI systems and features in relation to blockchain and IoT-enabled ecosystems.

![Figure_1](2022-07-04-Blockchain%20for%20AI%20Review%20and%20Open%20Research%20Challenge/Figure_1.png)

AI research 분야에선 자기 자신들을 “intelligent agents”에 대한 연구라고 규정한다.

즉, 환경을 인식하고 목표에 도달하기 위한 가능성을 최대화 하는 것을 말한다.

오늘날의 거의 대부분의 AI system들은 특정한 목표에 특화되어 있는 데이터셋을 결정을 내리기 위해 이용한다.

그러나, 많은 연구자들은 실생활에 긍정적으로 영향을 미칠, “진짜 똑똑한 결정”을 내릴 AI system를 만들기 위해 노력하고 있다.

Table 1은 AI apps에 관한 몇 가지의 새로운 트렌드와 그것의 장점을 보여준다.

...서술중...

**Reference**

1. Blockchain for AI: Review and Open Research Challenges, K. Salah et al., IEEE Access, 7, 10127-10149, 2019 ([https://ieeexplore.ieee.org/abstract/document/8598784](https://ieeexplore.ieee.org/abstract/document/8598784))