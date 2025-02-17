---
description: Zihan Zhou / Continuously Discovering Novel Strategies via Reward-Switching Policy Optimization / ICLR-2022  
---  

# **Title**

Continuously Discovering Novel Strategies via Reward-Switching Policy Optimization

## **1. Problem Definition**

강화학습을 이용하는 문제에서 정책의 다양한 집합들을 찾는 것은 매우 중요한 일이다. 더 나아가 다중 에이전트 환경에서 다수의 다양한 지역 최적을 확보하는 작업은 얼핏 보면 알 수 없었던 아예 새로운 행동 양상과 여러 개의 내쉬 균형을 찾음으로써 게임 환경 내에서 관측되지 않았던 에이전트들에게도 적용될 수 있는 강력한 정책들을 도출 할 수 있다. 이러한 근거로 강화학습에서 다양한 전략들을 확보하려는 연구가 다수 존재하는데, 대부분의 경우 다수의 정책들로 이루어진 집합을 평행하게 학습하는 방법으로 접근했다. 이와 같은 접근들은 추가로 내재적인 다양성 보상 및 보조 손실함수를 도입하여 학습 목적 함수를 달성한다. 그러나 강화학습 문제에 깔려있는 보상의 구조가 non-uniform한 경우, 위처럼 population 기반으로 확보한 정책들은 시각적으로 동일한 전략으로 귀결되는 경우가 많다. 또 다른 접근으로 multi-objective 최적화를 통해 보상 공간의 다양한 전략들을 직접적으로 탐색하거나 미리 정의된 목적 함수의 선형 조합을 무작위로 탐색하는 방법들도 있지만, 극소수의 특수한 문제 상황에서만 가능하다는 한계가 존재한다. 따라서 여기서는 기존의 방법들보다 간단하고 효과적이며, 범용적으로 쓰일 수 있는 학습 기법인 RSPO를 소개한다. RSPO는 강화학습 정책이 지역적으로 최적인 reference 정책들과 확연히 다른 해답으로 수렴하게끔 작동하는 필터링 기반의 목적 함수를 풀어서 새로운 전략을 얻는다. 이렇게 구한 새로운 전략은 다음 과정에서 또 다른 reference 정책이 되어 전체 최적화 과정에서 사용된다. 따라서 RSPO를 반복적으로 수행하면 적은 수의 시행만으로 다양한 전략들을 얻을 수 있다. 정책 최적화에 novelty 제약식을 엄격하게 적용하기 위해서, RSPO에서는 soft 목적 함수를 최적화하는 대신 rejection 샘플링을 사용한다. 특히 기준 정책에 따라 likelihood가 충분히 낮은 trajectory에 대해서만 외재적인 보상을 최적화한다. 한편 reference 정책과의 차이가 명확하지 않아 배제된 trajectoy를 추가로 활용하기 위해서 RSPO는 그러한 trajectory들에 대한 환경 보상을 무시하고 효과적인 탐색을 위해서 다양성 보상만을 최적화한다. 직관적으로, 전체 작업이 이렇게 학습의 목표를 외재적인 보상과 다양성 보상 사이에서 상황에 맞게 전환하므로 패러다임의 이름이 RSPO이다.
RSPO의 기여를 요약하면:

1. 다양한 정책들을 연속적으로 발견할 수 있는 새로운 알고리즘인 RSPO를 제안하였고, 이를 통해 효율적으로 다른 정책들을 발견할 수 있다.
2. 정책 최적화를 위해 cross-entropy 기반의 다양성 metric을 사용하고 다양성 중심 탐색을 촉진하기 위해 두 가지의 추가 다양성 중심 내적 보상을 사용할 것을 제안한다.
3. RSPO는 다양한 단일 에이전트 및 다중 에이전트 도메인에 걸쳐 일반적이고 효과적이다. 구체적으로, RSPO는 사전 지식 없이 사슴 사냥 게임 환경에서 최적의 정책을 학습한 최초의 알고리즘이며, 스타크래프트 환경에서 단 6번의 반복을 통해 시각적으로 분명하게 구별되는 6개의 승리 전략을 성공적으로 발견한다.

## **2. Motivation**

강화학습 분야에서 multi-modal 최적화 문제의 다양한 해를 구하는 방법으로 여러 블랙박스 기법이 제안됐다. 특히 가장 널리 쓰이는 패러다임은 population 기반의 multi-objective 최적화이다. 또한 반복적으로 정책들을 학습하는데에 집중한 연구들도 있다. 대표적으로 PSRO는 반복 과정을 통해 제로섬 게임에서 내쉬 균형을 갖는 전략들을 배우는 알고리즘이다. DIPG는 정책들 간의 최대평균차이를 학습 목적 함수로 설정하여 새로운 정책들을 배우고자 했다. RSPO는 복잡한 강화학습 환경에서 지역적으로 최적이며, 이미 존재하는 것들과 확연하게 다른 새로운 정책들을 반복적으로 찾아내어 다양한 전략들을 발견 할 수 있는 패러다임이다. 에이전트가 학습중인 정책이 이전에 발견되지 않은 지역 최적을 향해 지속적으로 수렴하게 만들기 위해서, RSPO는 최적화하는 과정 중 외재적인 보상과 내재적인 보상을 trajectory 기반의 novelty measurement를 통해 번갈아가며 사용한다. 구체적으로 trajectory가 이미 존재하는 정책들과 높은 likelihood를 갖는다면 RSPO가 exploration을 촉진하기 위해 내재적이고 다양한 보상을 통해 최적화를 수행하며, 이와 반대로 trajectory가 충분히 다르다면 외재적인 보상을 통해 평범한 최적화를 수행한다.

## **3. Method**

### 3.1 Preliminary

문제 환경은 MDP로 정의된다: $$M=\langle \mathcal{S, A},R,P,\gamma \rangle.$$ $$\mathcal{S}$$와 $$\mathcal{A}$$는 각각 상태 공간, 액션 공간을 의미하며, $$R(s,a)$$는 보상 함수이다. $$P(s^\prime|s,a)$$는 전이 행렬이고 $$\gamma$$는 할인 요소이다. 강화학습은 다음의 수식과 같이 정책을 최적화한다: $$J(\pi)=\mathbb{E}*{\tau\sim\pi}[\sum_t\gamma^tr_t].$$ 이때 $$\tau$$는 $$\{(s_t,a_t,r_t)\}$$로 나타내는 trajectory이고 정책 $$\pi$$는 trajectory에서 샘플링 된다. 물론 연구에서 풀고자 하는 문제는 에이전트가 여러 개인 다중 에이전트 문제이므로, $$\{\pi*{\theta^k}|1\le k \le M\}$$와 같은 다양한 M개의 정책들을 $$J(\theta)$$에 의해 지역적으로 최적화하면서도 차이 측정 지표인 $$D(\pi_{\theta^i},\pi_{\theta^j})$$로 평가했을 때 확연히 다르게 구분될 수 있게끔 알고리즘을 설계했다:

$$\underset{\theta^k}\max J(\theta^k)$$ $$\forall1\le k \le M,$$ subject to $$D(\pi_{\theta^i},\pi_{\theta^j})\ge\delta,$$ $$\forall 1 \le i <j \le M.\qquad(1)$$

### 3.2 Iterative Constrained Policy Optimization

위의 (1) 식을 직접 푸는 방법이 앞서 말한 population 기반의 학습 패러다임이다. 위의 문제를 직접 풀기 위해서 population 기반의 패러다임은 크기가 M인 큰 population을 필요로 한다. 그에 반해 RSPO는 k번째 반복 시행에서 단일 정책 $\pi_k$를 이전에 발견한 정책들 $$\pi_1,\dots,\pi_{k-1}$$과 충분히 다르게끔 제한하는 제약식 하에 최적화해서 새로운 전략 찾는 반복 과정을 적용한다. 반복 과정은 다음과 같이 나타낼 수 있다:

$$\theta_k=\underset{\theta}\argmax J(\theta),$$ subject to $$D(\pi_\theta,\pi_j)\ge\delta,$$ $$\forall1\le j <k.\qquad(2)$$

(2) 식은 population 기반의 목적 함수를 단일 정책에 대한 일반적인 최적화 문제로 축소하고, 크기가 M인 큰 population을 필요로 하지 않기 때문에 훨씬 풀기 쉽다. 그리고 차이 측정 지표 $$D$$의 thershold $$\delta$$를 적절하게 조절하여 (2) 식의 해가 제약식 $$D(\pi_\theta,\pi_j)=\delta$$의 경계면에 위치할 경우 지역 최적해가 아닐 수 있다는 문제를 해결했다. 보다 구체적으로, 차이 측정 지표 $$D$$를 설계하는데에 누적 cross-entropy를 도입했다:

$$D(\pi_i,\pi_j):=\mathcal{H}(\pi_i,\pi_j)=\mathbb{E}_{\tau\sim\pi_i}[-\underset{t}\sum\log\pi_j(a_t|s_t)].\qquad(3)$$

### 3.3 Trajectory Filtering for Enforcing Diversity Constraints

(2) 식과 같은 최적화 문제를 푸는데 널리 쓰이는 방법으로 제약 조건들을 학습 목적 함수의 페널티로 치환하는 라그랑지안 승수를 사용하는 접근이 있다. 즉 $$\beta_1,\dots,\beta_{k-1}$$을 하이퍼파라미터의 집합으로 설정하면 (2) 식의 soft 목적 함수는 다음과 같다:

$$J_{soft}(\theta):=J(\pi_\theta)+\underset{j=1}{\overset{k-1}\sum}\beta_jD(\pi_\theta,\pi_j).\qquad(4)$$

그러나 RSPO에서는 cross-entropy가 dense 함수이기 때문에 다양성에 대한 보너스를 목적 함수의 일부로 포함하는 작업이 원래 문제의 보상의 구조를 크게 변화시킬 수 있다. 따라서 라그랑지안 승수를 시간에 따라 강화하는 것이 필수적이다. 더 나아가 차이 측정 지표 $$D$$가 trajectory 샘플들에 대하여 추정되기 때문에 학습 목적 함수에 다소 큰 분산을 부과할 수도 있다. 이러한 부분들을 해결하기 위해 Trajectory 필터링을 제안한다. 이때 $$NLL(\tau;\pi)$$는 정책 $$\pi$$에 관한 trajectory $$\tau$$의 음의 log-likelihood를 뜻한다: $$NLL(\tau;\pi)=-\sum_{(s_t,a_t)\sim\tau}\log\pi(a_t|s_t).$$ 샘플 필터링은 각각의 reference 정책 $$\pi_j$$에 대한 NLL 값이 threshold $$\delta$$보다 크거나 같은 trajectory를 선별하는 방식으로 이루어진다.

$$J_{filter}(\theta)=\mathbb{E}*{\tau\sim\pi*\theta}[\phi(\tau)\underset{t}\sum\gamma^tr_t],$$ where $$\phi(\tau):=\underset{j=1}{\overset{k-1}\prod}\mathbb I[NLL(\tau;\pi_j)\ge\delta].\qquad(5)$$

### 3.4 Intrinsic Rewards for Diversity Exploration

(5) 식을 푸는 과정에서 주의해야 할 점은, trajectory 필터링을 통해 정책을 학습하는 과정의 초기에 상당수의 trajectory들을 배제 할 수도 있다는 것이다. 따라서 배제된 trajectory들을 사용할 수 있는가에 대한 의문에서 출발하여, RSPO는 추가적으로 배제된 trajectory들에 novelty-driven 목적 함수를 적용한다:

$$J_{switch}=\mathbb{E}*{\tau\sim\pi*\theta}[\phi(\tau)\underset{t}\sum\gamma^tr_t+\lambda\underset{j}\sum(1-\phi_j(\tau))NLL(\tau,\pi_j)].\qquad(6)$$

위의 목적 함수는 외재적인 보상을 최대화 하면서도 배제된 trajectory들의 cross-entropy를 최대화한다. 다음으로 다양성을 내포하는 탐색을 위해 두 가지 종류의 내재적인 보상을 제안하는데, (6) 식을 따르는 likelihood 기반의 보상과 새로운 상태와 보상에 도달하는 것에 집중하는 예측 기반의 보상이 있다:

$$r^{int}_B(a,s;\pi_j)=-\log\pi_j(a|s).\qquad(7)$$

$$r^{int}_R(s_t,a_t;\pi_j)=|f(s_t,a_t;\Psi_j)-r_t|^2.\qquad(8)$$

### 3.5 Reward-Switching Policy Optimization

RSPO 함수인 $$r_t^{RSPO}$$는 다음과 같이 나타낸다:

$$r^{RSPO}_t=\phi(\tau)r_t+\lambda\underset{j}\sum(1-\phi_j(\tau))r^{int}(a_t,s_t;\pi_j).\qquad(9)$$

이때 $$\lambda$$는 스케일링 하이퍼파라미터이다. 또한 외재적인 보상과 내재적인 보상은 상호 배타적인 관계로, trajectory $$\tau$$는 $$J_{filtering}$$에 포함되거나 탐색 보너스를 생산하기 위해 배제돼야 한다.

## **4. Experiment**

RSPO가 일반적인 강화학습 적용 분야에 사용될 수 있다는 것을 증명하기 위해, 크게 4개의 영역에서 실험을 진행하였다:

1. 단일 에이전트 환경(SPE),
  
2. 이중 에이전트 환경(Stag),
  
3. 연속 제어 환경(MuJoCo),
  
4. 다중 에이전트 환경(Starcraft).
  

### **Experiment setup**

강화학습 실험의 특성상 라벨링 된 학습 데이터 및 미리 생성하는 데이터셋이 따로 존재하지 않고 에이전트가 환경을 관측하며 얻는 보상을 통해 학습이 진행된다. 4개의 영역에서 진행한 실험의 baseline 알고리즘은 다음과 같다:

- ‘PPO with restarts(PG)’,
  
- ‘Diversity-Inducing Policy Gradient(DIPG)’,
  
- ‘population-based training with cross-entropy(PBT-CE)’,
  
- ‘DvD’,
  
- ‘SMERL’,
  
- ‘Random Netwalk Distillation(RND)’.
  

또한 실험 환경마다 에이전트가 달성해야 하는 작업의 목적이 조금씩 다르고, 연구의 취지가 에이전트가 얼마나 새로운 전략을 잘 찾는지 확인하는 것인만큼 일정하게 통일되어 정형화 된 평가 지표를 도입하지는 않았지만 각 실험마다 해당 실험 환경에서 RSPO가 baseline 알고리즘들 대비 어느정도 우수한 성능을 보였는지 알 수 있는 자료들을 정리했다.

### **4.1 Single-Agent Particle-World Environment**
![1](/.gitbook/2022-spring-assets/heemang_park_2/1.png)
첫 번째 실험은 희박한 보상이 주어졌을 때 목적지를 찾아가야 하는 4-Goals 시나리오에서 진행됐다. 에이전트는 중앙에서 출발하고 랜드마크를 찾으면 보상을 획득한다. 시나리오에 총 3가지 난이도가 있는데, 쉬운 난이도는 랜드마크의 위치가 고정되어 있고, 중간 난이도는 랜드마크가 무작위로 분포하고, 어려운 난이도는 랜드마크의 위치와 크기가 무작위로 설정된다. Figure 2(a)에 알고리즘이 발견한 서로 다른 종류의 전략들의 수가 정리됐고, Figure 2(b)에 어려운 난이도에서 알고리즘의 에이전트가 얻은 보상이 정리됐다. 주목할만한 점은 RSPO가 유일하게 어려운 난이도에서 랜드마크로 도달할 수 있는 전략을 찾은 알고리즘이라는 것이다.
![2](/.gitbook/2022-spring-assets/heemang_park_2/2.png)

### 4.2 2-Agent Markov Stag-Hunt Games

두 번째 실험은 좌표 공간 상에 두 개의 에이전트와 두 개의 사과와 한 개의 괴물이 존재하는 Stag-Hunt game에서 진행됐다. 보상을 얻는 방법은 다음과 같다: 만약 한 개의 에이전트가 괴물을 만난다면 -2의 보상을 얻고, 두 개의 에이전트가 동시에 괴물을 만난다면 괴물을 잡고 +5의 보상을 얻고, 에이전트가 사과에 도달하면 +2의 보상을 얻는다. Figure 4는 20번의 반복 시행 동안 RSPO가 발견한 모든 전략들을 나타낸다. Figure 5는 내재적인 보상이 없을 때 학습이 이루어지는 동안 수락된 trajectory의 비율이 낮다는 것을 보여준다.
![3](/.gitbook/2022-spring-assets/heemang_park_2/3.png)
![4](/.gitbook/2022-spring-assets/heemang_park_2/4.png)
![5](/.gitbook/2022-spring-assets/heemang_park_2/5.png)

### 4.3 Continuous Control in MuJoCo

세 번째 실험은 MuJoCo 시뮬레이터에 있는 Half-Cheetah, Hopper, Walker2d, Humanoid 모델에서 RSPO와 baseline의 성능 비교를 통해 진행됐다. 결과는 Table 2에 정리됐는데, RSPO가 여러 모델들에서 baseline들에 비해 우수한 성능을 보인다. 특히 시뮬레이터에서 확인할 수 있는 에이전트의 움직임이 baseline들과 확연하게 다른 RSPO의 결과를 통해 성공적으로 새로운 전략을 얻었다는 것을 알 수 있다.

### 4.4 Starcraft Multi-Agent Challenge

마지막 실험은 SMAC에 포함된 쉬운 난이도의 2m_vs_1z와 어려운 난이도의 2c_vs_64zg 환경에서 진행됐다. 결과의 비교에 육안으로 확인 할 수 있는 다른 전략들의 수를 이용했는데, 두 난이도의 환경 모두에서 RSPO가 가장 많은 수의 서로 다른 전략을 발견했다는 것을 알 수 있다.
![6](/.gitbook/2022-spring-assets/heemang_park_2/6.png)
![7](/.gitbook/2022-spring-assets/heemang_park_2/7.png)
![8](/.gitbook/2022-spring-assets/heemang_park_2/8.png)

## **5. Conclusion**

RSPO라는 간단하고 범용적이며, 효과적인 반복 학습 알고리즘을 통해 강화학습에서 새로운 전략들을 발견할 수 있다. 외재적인 보상과 내재적인 보상을 번갈아가며 사용하는 기법을 통해서 기존의 연구에서 사용한 방법들이 목적 함수를 푸는데에 있는 여러 제한을 극복했다. 또한 실험적으로 RSPO는 폭넓은 강화학습 분야의 실험 환경에 적용되어 성공적으로 새로운 전략들을 잘 찾았다. 몇 가지 이론적인 부분과 sample efficiency에 대한 문제들을 이후의 연구에서 해결해서 보충할 수 있을 것 같다.

---

## **Author Information**

- Heemang Park
  - SILAB, KAIST
  - Research Topic: MARL, Competitive Game

## **6. Reference & Additional materials**

- GIF Demonstrations on MuJoCo and SMAC:
  - https://sites.google.com/view/rspo-iclr-2022
- Baseline Github Implementations:
  - https://github.com/openai/multiagent-particle-envs
  - https://github.com/dtak/DIPG-public
  - https://github.com/AnujMahajanOxf/MAVEN
  - https://github.com/ollenilsson19/PGA-MAP-Elites
  - https://github.com/jparkerholder/DvD_ES
