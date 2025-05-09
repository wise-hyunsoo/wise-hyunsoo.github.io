---
theme: beam
footer: May 20th, 2025
header: Wise (AI Lab, Vision)
paginate: "True"
style: "|  .columns {    display: flex;    grid-template-columns: repeat(2, minmax(0, 1fr));    gap: 1rem;  }  .columns-left {    background: white;  }  .columns-right {    background: white;  }"
---
<!-- _class: title -->
# Diffusion Meets Flow Matching:
Wise
AI Lab
2025-05-20

---
<!-- _class: tinytext -->
# Contents
- Mathematical preliminaries
	- Integration
	- ODE vs SDE
	- ODE sampler
	- SDE sampler
- Diffusion models and Flow matching
	- Diffusion models
	- Value-based / Policy-based / Model-based RL
- Sampling
	- DDIM sampling
	- Flow matching sampling
	- Probability Flow ODE sampling

---
<!-- _class: tinytext -->
# Mathematical preliminaries
## Integration (in $\mathbb{R}$ and $\mathbb{R}^n$)
### Riemann Integral

<p align="center">

<img src="attachments/Pasted%20image%2020250509121540.png" width="350"/>
</p>

### Riemann-Stieltjes Integral
- Bounded variation and Riemann-Stieltjes Sum
$$
BV(f,[a,b]) := \sup_{P} \bigg( \sum_{i=1}^n \Big|f(x_i)-f(x_{i-1})\Big| \bigg)
$$
$$
S(f,P,g) := \sum_{i=1}^{n} f(x_i^*) (g(x_i)-g(x_{i-1}))
$$

---
# Preliminaries
## Integration on manifolds

![Pasted image 20250509133954.png](attachments/Pasted%20image%2020250509133954.png)

Goal: Calculate $\mathbb{E}_{x\sim p(x)} [f(x)]$ 
(Hint: [Law of Large Numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers))

---
# Pre
## Manifolds
> Note (Wikipedia - Manifold)
![Pasted image 20250509134627.png](attachments/Pasted%20image%2020250509134627.png)

---
# Pre
## Manifolds

<div class="columns">
<div class="columns-left">

![Pasted image 20250509141840.png](attachments/Pasted%20image%2020250509141840.png)
</div>
<div class="columns-right">

![Pasted image 20250509141814.png](attachments/Pasted%20image%2020250509141814.png)
</div>
</div>

---
# Preliminaries
## Integration on manifolds

<p align="center">

<img src="attachments/Pasted%20image%2020250509134435.png" width="800"/>
</p>

$$
\int_{U \subset \mathcal{M}} \omega = \int_{\varphi(U)} (\varphi^{-1})^* \omega
$$

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

- test
- wow
- great
- gg
- wownae
</div>
<div class="columns-right">

wow
</div>
</div>


---



---
# Overview of RL
## GAE
![Pasted image 20250220134516.png](attachments/Pasted%20image%2020250220134516.png)

---
# Overview of RL
## Objective function of RL

$$L(\theta) = \mathbb{E}_{s_0\sim p_0 (s)}[V_{\pi_\theta}(s_0)]$$
- policy를 neural network의 parameter $\theta$를 도입하여 위의 목적함수를 최대화하도록 훈련해서 optimal policy를 얻고자 하는 것이 RL의 목표

---
<!-- _class: tinytext -->
# Overview of RL
- Value-based RL
	- Q functiond을 학습하여 $\pi(s) = \arg \max_{a\in \mathcal{A}} Q(s,a)$를 policy로 사용
	- 단점
		- function approximation (such as neural networks)
		- bootstrapped value function estimation (TD-like method)
		- off-policy learning
		- This combination : `the deadly triad`
		- RL 알고리즘 불안정함
- Policy-based RL
	- Policy search method
	- 대부분 policy gradient 방법론 사용
- Model-based RL (MBRL)


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