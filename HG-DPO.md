SFT가 이전 stage에서의 학습된 knowledge를 잊어버린다는 것이 흥미로웠는데, 이 연구에서 지칭하는 Supervised Fine-Tuning(SFT)에서 구체적으로 training parameter에 대해 궁금한 점이 있어서 질문을 남겼습니다.

1. Appendix에서 ϵ_base 가 SD1.5 기반의 majicmix-v7 모델을 backbone모델로 두고 이를 SFT했다고 하는데, 혹시 이 SFT에서의 training parameter가 backbone모델의 full-parameter인지 아니면 LoRA parameter일까요?
2. Table 2.의 결과의 맨 마지막 3행의 결과들(+SFT 결과들)에서 이 SFT는 ϵ_base의 기존 parameter는 freeze하고 LoRA parameter만 SFT를 한 것일까요?