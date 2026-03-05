

1. 인턴 경력 기술서
- 이 연구는 벤치마크 제작에 대한 연구인가요?
- MLLM-as-a-Judge 벤치마크에서 모델의 bias가 있다고 하셨는데, 여기서 말하는 bias가 어떤 의미인지? modality에 bias된 응답, 일부 modality를 무시하고 응답을 한다는 의미인지? 이를 개선하기 위해 Chain-of-judgement를 사용하셨는데 CoJ는 어떤걸 말하는 건지? Reasoning LLM에서 Chain-of-thought을 통해서 응답의 reasoning capability를 개선한것과 유사한 느낌으로, 모델이 뭔가 계속 판단의 sequence를 스스로 생성해서 판단을 이어나가는건지? 아니면 뭔가 predefined checklist를 줘서 모델이 그 checklist에 맞게 판단을 이어나가는 건지?

1. ICLR
- 이 연구를 text-to-image diffusion 모델에서 

2. ICCVW
- Masked image modelling에서 Implicit neural representation을 이용한 연구인것 같은데, Implicit neural representation이 그 특성 자체가 좌표에서 pixel값을 내뱉는 그런 모델이어서, arbitrary resolution에 대해서 출력이 가능하다는 특징이 있다고 알고 있는데, 혹시 이 연구에서 제안한 Masked Implicit Neural representation에서도 이런식으로 고해상도 이미지를 생성 혹은 복원하는데 사용할 수 있을까요? 

1. bootcamp ai tech
- 일종의 강의 조교 활동 인지? 강의하는 분이 따로 있고, 그분을 support하는 역할인지? 아니면 주도적으로 강의내용을 기획하고 강의자료를 작성하고, 그 자료를 바탕으로 직접 강의도 하는 역할인지?

- **학습 진도를 따라가지 못하거나 동기부여가 떨어진 수강생을 마주했을 때, 그분들에게 힘이 되기 위해 개인적으로 노력해보신 경험**이 있으신가요? 그때 어떤 마음으로 다가가셨고, 지원자님의 행동이 상대방에게 어떤 영향을 주었나요?



%% - 강의 자료 제작 및 대회 운영 이면, 혹시 여러명이서 같이 강의자료 제작을 하거나 대회운영을 같이 하는지?
- 만일 그렇다면 이런 강의조교활동을 여러명의 조교님들과 함께 협어하는 과정에서  %%




지금 하려는건 강화학습인데, task는 다음과 같아. 

[Task 정보]
- Reference 밑색 이미지 A와 검은색 선과 흰배경으로 이루어진 선화이미지 B가 주어졌을때, A를 참고해서 B의 밑색을 칠하는 칠하는 Task
- 그런데 그 밑색을 칠하는 과정은 다음과 같은 단계로 진행되어야함
- 먼저 A의 밑색으로 이루어진 Scribble 이미지를 모델로 부터 얻고, 그 Scribble 이미지가 LazyBrush라는 알고리즘을 통해, 선화 B에 대한 밑색이 최종적으로 완성
- 구체적으로 Scribble 이미지는 https://huggingface.co/Qwen/Qwen3.5-27B 와 같은 VLM을 이용해서, Scribble 이미지로 렌더링 될 수 있는 텍스트(string)을 출력하게 하고, 그 텍스트로부터 Scribble 이미지를 만들고, 그 이미지와 선화 B로 밑색이 완성되도록 해야함.
- Scribble로 렌더링 될 수 있도록 하기 위해, [3차 베지에 곡선의 정보들과 그 곡선의 두께 및 색상정보] 들의 집합으로 표현할 수 있게 VLM의 출력 구성을 해줘.
- 또한 Qwen VLM 들이 https://github.com/QwenLM/Qwen3-VL/blob/main/qwen-vl-finetune/tools/process_bbox.ipynb 와 같은 Box 좌표를 활용하는 것도 참고해서 구현해주면 좋을 것 같아.

[모델의 대략적 제안]
- https://huggingface.co/Qwen/Qwen3.5-27B 와 같은 VLM으로 Action을 내뱉어야 함
- 훈련은 오직 RL, 예를들어 GRPO로만 가지고 되었으면 좋겠음
- Reward는 B의 실제 완성 밑색 이미지와, 이 과정을 거쳐서 나온 밑색이미지(Scribble 이미지의 LazyBrush 결과)와의 Negative MSELoss 혹은 Negative L1 Loss로 구성하려고 해.
- RL을 위해서 TRL이나 VeRL과 같은 강화학습 framework를 사용하고, Rollout을 위해 vllm을 꼭 사용하면 좋을 것 같아.

[action의 개선 관련] 
- "그림 그리는 Action", 즉 3차 베지에 곡선 정보, 곡선 두께, 색상정보 외에도, action 중에 레퍼런스 이미지 A의 실제 밑색 RGB값을 찍어서 VLM 모델이 직접 확인하는 Action도 들어가면 좋을 것 같아.
- 또한, 모델 상 성능향상에 필요하다면 입력 이미지 A, B 말고도 A나 B의 Crop을 알아서 해서 확대된 이미지도 VLM이 볼 수 있도록 action을 구성해도 좋을 것 같아.

---
위 내용과 관련해서 Dataset 설계 및 모델 구조와 action의 형태를 구체적으로 다듬어서 코드로 작성해줘. 