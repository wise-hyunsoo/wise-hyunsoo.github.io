# Note 1

Example: link to [Mermaid Diagrams](../Features/Mermaid%20Diagrams.md) under `Features`


$$
\begin{align*}
\tilde{\mu} &= \dfrac{\sqrt{\alpha_t}(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t} x_t + \dfrac{\sqrt{\bar{\alpha}_{t-1}}\beta_t}{1-\bar{\alpha}_t}\dfrac{1}{\sqrt{\bar{\alpha}_t}}(x_t-\sqrt{1-\bar{\alpha}_t}\epsilon_t) \\

&= \dfrac{(\alpha_t-\bar{\alpha}_{t})}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}x_t + \dfrac{\beta_t}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}(x_t-\sqrt{1-\bar{\alpha}_t}\epsilon_t) \\


&= \dfrac{(\alpha_t-\bar{\alpha}_{t})}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}x_t + \dfrac{1-\alpha_t}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}(x_t-\sqrt{1-\bar{\alpha}_t}\epsilon_t) \ (\because \beta_t = 1-\alpha_t)\\

&= \dfrac{1}{\sqrt{\alpha_t}}x_t-\dfrac{1-\alpha_t}{\sqrt{\alpha_t}\sqrt{1-\bar{\alpha}_t}}\epsilon_t \\

&= \dfrac{1}{\sqrt{\alpha_t}}\bigg(x_t-\dfrac{1-\alpha_t}{\sqrt{1-\bar{\alpha}_t}}\epsilon_t\bigg) \\

\end{align*}

$$