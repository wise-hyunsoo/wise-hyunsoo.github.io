---
theme: beam
footer: February 20th, 2025
header: Wise (AI Lab, Vision)
paginate: "True"
---
<!-- _class: title -->
# Title of the presentation
Author's name
University of XYZ
2022-06-19

---
<!-- _class: tinytext -->
# Contents
- Preliminaries
	- Random variables
	- Monte Carlo estimation
- Overview of RL
	- Terminologies
	- Value-based / Policy-based / Model-based RL
- Policy Optimization
	- Policy Gradient Theorem
	- REINFORCE & GAE
	- PPO
- Reinforcement Learning with LLMs
- Reinforcement Learning with Verifiable Rewards
- How to apply RL to Colorization(밑색/채색)
---

# Preliminaries
## Random variables
$X: \Omega \rightarrow E$
$(\Omega, \Sigma, P)$: 확률 공간 (Sample space $\Omega$, Event space $\Sigma$, Probability measure $P$)
(with some assumptions), we also have
### Probability density function
$\mathbb{E}_P [f(X)]:=\int_\Omega f(X(\omega))dP(\omega)=\int_E f(x)p(x)dx=:\mathbb{E}_{x\sim p(x)} [f(x)]$

> Note
> $E$ might be complex..


---
# Preliminaries
## Monte Carlo estimation
Goal: Calculate $\mathbb{E}_{x\sim p(x)} [f(x)]$ 
(Hint: [Law of Large Numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers))

---
# Overview of RL
![bg right width:700px](attachments/Pasted%20image%2020250220122200.png)

## Terminologies
- Agent
- Environment
- $\mathcal{S}$: a finite set of states (상태 집합)
- $\mathcal{A}$: a finite set of actions (행동 집합)
- Policy $\pi: \mathcal{A}\times \mathcal{S}\rightarrow [0,1]$
	- Optimal policy
		- $\pi^* = \arg \max_{\pi} \mathbb{E}_{s_0\sim p_0 (s)} [V_\pi (s_0) ]$
		- $V_{\pi^*} (s) \geq V_{\pi} (s) \ (\forall x\in \mathcal{S}, \forall \pi)$
- Reward $R:\mathcal{A}\times \mathcal{S}\rightarrow \mathbb{R}$
- Value function $V:\mathcal{S}\rightarrow \mathbb{R}$
- Q-function $Q:\mathcal{S}\times \mathcal{A}\rightarrow \mathbb{R}$ 

---
# Overview of RL
## ## Terminologies
- Advantage function $A:\mathcal{S}\times \mathcal{A}\rightarrow \mathbb{R}$
	- $A(s,a):=Q(s,a)-V(s)$
- Generalized advantage estimation (GAE)
	- Advantage function을 계산하려면 state와 action 값이 필요하다.
	- 그런데 이러한 state, action은 (policy와 initial state의 확률분포에 depend하는) random variable이다.
	- 따라서 Advantage function의 evaluation 결과 $A(s,a)$도 random variable
	- 이 random variable을 estimate하기 위해 $R, V$를 통해 Monte Carlo estimate을 하는데 그 estimation의 variance를 줄이기 위해 나온 방법이 GAE


---

## Slides
- wow
- hull
- great