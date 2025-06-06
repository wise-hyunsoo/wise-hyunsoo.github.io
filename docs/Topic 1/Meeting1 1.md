---
theme: beam
footer: May 26th, 2025
header: Wise (AI Lab, Kakao Entertainment)
paginate: "True"
style: "|  .columns {    display: flex;    grid-template-columns: repeat(2, minmax(0, 1fr));    gap: 1rem;  }  .columns-left {    background: white;  }  .columns-right {    background: white;  }"
---
<!-- _class: title -->
# Reinforcement Learning
Wise
AI Lab
2025-05-26

---
<!-- _class: tinytext -->
# Contents
- Preliminaries
	- Random variables
	- Monte Carlo estimation
- Overview of RL
	- Terminologies
	- Value-based / Policy-based / Model-based RL
- Value-based RL
	- Policy iteration
- Policy Optimization
	- Policy Gradient Theorem
	- REINFORCE
- Reinforcement Learning with LLMs
- Reinforcement Learning with Verifiable Rewards
- Appendix
---
# Main Question

## How are Supervised Fine-Tuning and Reinforcement Learning different?

---
# Preliminaries
## Random variables
$X: \Omega \rightarrow E$
$(\Omega, \Sigma, P)$: 확률 공간 (Sample space $\Omega$, Event space $\Sigma$, Probability measure $P$)
(with some assumptions), we also have
### Probability density function
$\mathbb{E}_P [f(X)]:=\int_\Omega f(X(\omega))dP(\omega)=\int_E f(x)p(x)dx=:\mathbb{E}_{x\sim p(x)} [f(x)]$

> Note
> $E$ might be complex.. sequences.. $\ell_p$ spaces.. ($1\leq p \leq \infty$)

---
# Preliminaries
## Monte Carlo estimation
Goal: Calculate $\mathbb{E}_{x\sim p(x)} [f(x)]$ 
(Hint: [Law of Large Numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers))

---
<!-- _class: tinytext -->
# Overview of RL
<div class="columns">
<div class="columns-left">

<br> <br>
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

</div>
<div class="columns-right">

<br> <br>
![width:800px](attachments/Pasted%20image%2020250220122200.png)

</div>
</div>



---
# Overview of RL
<div class="columns">
<div class="columns-left">

- Agent - Environment - $\mathcal{S}$: a finite set of states (상태 집합)
- $\mathcal{A}$: a finite set of actions (행동 집합)
- Policy $\pi: \mathcal{A}\times \mathcal{S}\rightarrow [0,1]$
- Optimal policy 
- $\pi^* = \arg \max_{\pi} \mathbb{E}_{s_0\sim p_0 (s)} [V_\pi (s_0) ]$ - $V_{\pi^*} (s) \geq V_{\pi} (s) \ (\forall x\in \mathcal{S}, \forall \pi)$ 
- Reward $R:\mathcal{A}\times \mathcal{S}\rightarrow \mathbb{R}$ 
- Value function $V:\mathcal{S}\rightarrow \mathbb{R}$ 
- Q-function $Q:\mathcal{S}\times \mathcal{A}\rightarrow \mathbb{R}$

</div>
<div class="columns-right">

![width:800px](attachments/Pasted%20image%2020250526091826.png)

</div>
</div>


---
# Overview of RL

## Value function, Q-function, and Reward

- $V_\pi (s) := \mathbb{E}_\pi [\sum_{t=0}^\infty \gamma^t r_t | s_0 =s]$
- $Q_\pi (s,a) := \mathbb{E}_\pi [ \sum_{t=0}^\infty \gamma^t r_t | s_0=s, a_0=a]$
- Reward $r_t:= R(s_t,a_t)$
## Objective function of RL

$$L(\theta) = \mathbb{E}_{s_0\sim p_0 (s)}[V_{\pi_\theta}(s_0)]$$
- policy를 neural network의 parameter $\theta$를 도입하여 위의 목적함수를 최대화하도록 훈련해서 optimal policy를 얻고자 하는 것이 RL의 목표
- Optimal policy $\pi*=\arg \max_\pi \mathbb{E}_{p_0(s_0)} [ V_\pi (s_0)]$


---
<!-- _class: tinytext -->
# Overview of RL
- Value-based RL
	- Q function을 학습하여 $\pi(s) = \arg \max_{a\in \mathcal{A}} Q(s,a)$를 policy로 사용
	- World model을 알 때, 즉 state transition $p_S(s'|s,a)$를 알 때와, 모를 때로 나눠서 접근
	-  단점
		- function approximation (such as neural networks)
		- bootstrapped value function estimation (TD-like method)
		- off-policy learning
		- RL 알고리즘 불안정함
- Policy-based RL
	- Policy search method
	- 대부분 policy gradient 방법론 사용
- Model-based RL (MBRL)


--- 
# Overview of RL
## Classification of RL
![Pasted image 20250220135415.png](attachments/Pasted%20image%2020250220135415.png)

---
# Value-based RL
## Policy iteration
- Bellman’s optimality equations
	- $V^* (s) = \max_a \Bigg[ R(s,a)+\gamma \mathbb{E}_{p_S (s'|s,a)}[V^* (s')]  \Bigg]$
- Bellman operator
	- $\mathcal{B} V(s):= \max_a \Bigg[ R(s,a)+\gamma \mathbb{E}_{T(s'|s,a)}[V(s')]\Bigg]$
- Contraction mapping theorem

- $\pi' (s)=\arg \max_a \bigg[ R(s,a)+ \gamma \mathbb{E}[V_\pi (s')] \bigg]$

---
# Value-based RL
## Policy iteration

![width:1000px center](attachments/Pasted%20image%2020250526114429.png)

---
# Policy Optimization
## Policy Gradient Theorem
![Pasted image 20250220135043.png](attachments/Pasted%20image%2020250220135043.png)

---

# Policy Optimization
## REINFORCE
- Monte Carlo version
![Pasted image 20250220135800.png](attachments/Pasted%20image%2020250220135800.png)
- 단점: estimation $G_t$의 분산이 큼 
	- 해결책: baseline을 사용
	- Any function that satisfies $\mathbb{E}[\nabla_\theta b(s)]=0$ is a valid baseline.
- 결과적으로 $\nabla_\theta J(\theta) = \mathbb{E}_{\rho_\theta (s) \pi_\theta(a|s)} [(Q_{\pi_\theta}(s,a)-b(s))\nabla_\theta \log \pi_\theta (a|s)]$

---
# Policy optimization
## Variance reduction in REINFORCE
- 결과적으로 $\nabla_\theta J(\theta) = \mathbb{E}_{\rho_\theta (s) \pi_\theta(a|s)} [(Q_{\pi_\theta}(s,a)-b(s))\nabla_\theta \log \pi_\theta (a|s)]$

![w:650 center](attachments/Pasted%20image%2020250526105114.png)

![Pasted image 20250526105221.png](attachments/Pasted%20image%2020250526105221.png)

---
# Policy Optimization
- PPO, DPO, GRPO ...
- [DPO vs. PPO](https://kakaoent.atlassian.net/wiki/spaces/RTC/pages/3772907636/DPO+vs.+PPO)
- [More on PPO](https://kakaoent.atlassian.net/wiki/spaces/RTC/pages/3773169715/More+on+PPO)
- [GRPO and Insights of RL](https://kakaoent.atlassian.net/wiki/spaces/RTC/pages/3782509435/GRPO+and+Insights+of+RL)


 ![width:900px center](attachments/Pasted%20image%2020250526111209.png)

---
# Reinforcement Learning with LLMs
- InstructGPT
![width:650px center](attachments/Pasted%20image%2020250220144832.png)


---
# RL with LLMs
- ChatGPT
![width:800px center](attachments/chatgpt_diagram_light.png)


---
# RL with LLMs

![Pasted image 20250220145451.png](attachments/Pasted%20image%2020250220145451.png)

---
# RL with LLMs
- DeepSeek-R1-Zero
	- RL on the Base Model
![width:500px center](attachments/Pasted%20image%2020250220145739.png)

---
# Reinforcement Learning with Verifiable Rewards
## RLVR
![width:900px center](attachments/Pasted%20image%2020250220150025.png)

---
# RLVR
- Tulu 3
![Pasted image 20250220150058.png](attachments/Pasted%20image%2020250220150058.png)


---
# Appendix

- [SFT Memorizes, RL Generalizes](https://kakaoent.atlassian.net/wiki/x/BYPv4Q)
![width:600px center](attachments/Pasted%20image%2020250526105837.png)

---
# Appendix
- [SFT Memorizes, RL Generalizes](https://kakaoent.atlassian.net/wiki/x/BYPv4Q)

![width:600px center](attachments/Pasted%20image%2020250526112856.png)

---
# Appendix

- Reinforcement Learning Finetunes Small Subnetworks in Large Language Models
![width:800px center](attachments/Pasted%20image%2020250526110002.png)

---
# Appendix
 - Reinforcement Learning Finetunes Small Subnetworks in Large Language Models
 
![width:900px center](attachments/Pasted%20image%2020250526112505.png)


---
# Appendix
- Reinforcement Learning Finetunes Small Subnetworks in Large Language Models

![width:600px center](attachments/Pasted%20image%2020250526112555.png)