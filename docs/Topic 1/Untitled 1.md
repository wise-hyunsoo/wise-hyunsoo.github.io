---
marp: true
theme: beam
size: 4:3
title: Marp custom themes
paginate: "True"
---
<!-- _class: title -->

# A very long title of my beamer-esque presentation
<br/>

Author's name
University of XYZ
2022-26-03
(Only normal text is centered now)

---

# A normal slide



---
# Title page ad hoc fix

- If the title of your presentation is too long and the border intersects with the text underneath, use the following

```html
# Title
<br/>
<!-- empty line here --->
Author's name
University of XYZ
...
```
- make sure to leave an empty line below the `<br/>` tag

---
<!-- _class: tinytext -->
# Tinytext class

- use `<!-- _class: tinytext -->` to make some text tiny
- might be useful for References


현재 선화에 stroke를 해야하는 유력한 좌표들을 추출하는 코드를 아래에 첨부하는데, 여기에 있는 xs_sorted_clustered 와 ys_sorted_clustered 의 좌표들을 "paint dots"의 target_x, target_y로 사용하도록 하고 싶어. 즉, 모델은 target_x, target_y의 정보는 아래 코드로 미리 이용할 수 있게하고, 모델이 내부적으로 판단해서 어떤 src_x, src_y를 잘 판단해서 stroke를 할 수 있도록, 코드를 전반적으로 수정해 줄 수 있을까?


vllm을 통해 양질의 적절한 length의 rollout 데이터를 뽑는 과정이 필요해서 그런데, 현재 일단 base mode(Qwen3.5-27B)로, 일종의 off-policy형태로, 결과가 잘나오는 rollout 데이터만 고르고 싶은데 다음과 같이 접근하는게 가능한가?

- 현재 강화학습에서 하려는 task를 간략히 얘기하면, 주어진 이미지 2개(하나는 선화, 다른 하나는 선화의 다른 구도의 레퍼런스 밑색이미지)를 받아서, 선화의 요청한 point에서의 적절한 RGB값을 잘 찾는게 목표야.

- 현재 Tool이 모델이 출력한 좌표값을 입력으로 받는 fine_paint과 미리 추출한 선화의 branch target point에서 palette를 선책하는 paint라는게 있고, 주어진 좌표에 대해 적절한 bbox좌표를 입력하는 zoom_in_line 이라는게 있고, 현재까지 estimate된 RGB값을 바탕으로 선화에 RGB를 입혀서 선화를 색칠해서 rendering 하는 check라는 tool이 있어. (즉, check라는 tool은 입력은 없이 출력으로 선화에 대한 색칠 결과가 출력되는것임)

- 이 상황에서 fine_paint, paint, zoom_in_line, check 등을 적절히 사용하는데, check 했을때 결과가 이상하면 모델이 다시 zoom_in이나 rgb_estimate 등을 해서 다시 예측해서 수정하도록 해서, 최종 결과가 좋은 rollout 결과를 뽑고 싶어.

- 그런데 위와 같이 잘못했을때 다시 수행하는식으로 rollout 데이터가 뽑히면 너무 response length가 길어저서 모델 훈련때 gpu 메모리 이슈등 문제가 생겨서, 잘못된 rollout 토큰부분은 지우고(잘라내고), 잘된부분만 이어붙여서 좋은 rollout 데이터들을 만들고 싶은 상황이야.

- 현재는 내가 알기로 막 20개의 target point에 대해 먼저 paint tool을 이용해서 20개의 palette를 모델이 예측하도록 했는데, 뭔가 결과가 좋지않아서 지금 양질의 rollout 데이터를 만들때에는 target 포인트 딱하나마다 zoom_in을 같이 사용하던가 해서 palette 추출을 하는식으로 정확한 rollout data를 추출하고 싶어.

- 물론 length가 길어지면 vram 메모리 이슈가 있으니, 만일 모델이 잘못 예측한건 rollout 중간에 버리던가 masking하는 식으로해서 계속 양질의 출력을 할 수 있으면 좋을 것 같아.  

위와 같은 접근으로 강화학습 혹은 SFT로 사용할만한 좋은 rollout 데이터를 뽑는게 가능할까?


현재 train_verl.sh을 통해 강화학습을 기획중인데, 강화학습에 맞춰서 현재 colorization_tool.py를 활용해서 off-policy rollout data를 vllm으로 뽑아 보고 싶은데, 그전에 제일 먼저 현재 강화학습이 어떻게 구성되었는지,        
reward는 뭔지, tool은 뭔지, 어떤 task를 하는중인지, system prompt는 뭔지 등을 종합적으로 보고 먼저 현재 상황을 파악해줘.   


이제 아래에 대해 답변 및 제안 해줘. vllm을 통해 양질의 적절한 length의 rollout 데이터를 뽑는 과정이 필요해서 그런데, 현재 일단 base mode(Qwen3.5-27B)로, 일종의 off-policy형태로, 결과가 잘나오는 rollout 데이터만 고르고 
 싶은데 다음과 같이 접근하는게 가능한가?                                                                                                                                                                                    
                                                                                                                                                                                                                            
- 현재 강화학습에서 하려는 task를 간략히 얘기하면, 주어진 이미지 2개(하나는 선화, 다른 하나는 선화의 다른 구도의 레퍼런스 밑색이미지)를 받아서, 선화의 요청한 point에서의 적절한 RGB값을 잘 찾는게 목표야.                  
                                                                                                                                                                                                                            
- 현재 Tool이 모델이 출력한 좌표값을 입력으로 받는 fine_paint과 미리 추출한 선화의 branch target point에서 palette를 선책하는 paint라는게 있고, 주어진 좌표에 대해 적절한 bbox좌표를 입력하는 zoom_in_line 이라는게 있고,   
현재까지 estimate된 RGB값을 바탕으로 선화에 RGB를 입혀서 선화를 색칠해서 rendering 하는 check라는 tool이 있어. (즉, check라는 tool은 입력은 없이 출력으로 선화에 대한 색칠 결과가 출력되는것임)                             
                                                                                                                                                                                                                            
- 이 상황에서 fine_paint, paint, zoom_in_line, check 등을 적절히 사용하는데, check 했을때 결과가 이상하면 모델이 다시 zoom_in이나 rgb_estimate 등을 해서 다시 예측해서 수정하도록 해서, 최종 결과가 좋은 rollout 결과를     
뽑고 싶어.                                                                                                                                                                                                                  
                                                                                                                                                                                                                            
- 그런데 위와 같이 잘못했을때 다시 수행하는식으로 rollout 데이터가 뽑히면 너무 response length가 길어저서 모델 훈련때 gpu 메모리 이슈등 문제가 생겨서, 잘못된 rollout 토큰부분은 지우고(잘라내고), 잘된부분만 이어붙여서    
좋은 rollout 데이터들을 만들고 싶은 상황이야.                                                                                                                                                                               
                                                                                                                                                                                                                            
- 현재는 내가 알기로 막 20개의 target point에 대해 먼저 paint tool을 이용해서 20개의 palette를 모델이 예측하도록 했는데, 뭔가 결과가 좋지않아서 지금 양질의 rollout 데이터를 만들때에는 target 포인트 딱하나마다 zoom_in을  
같이 사용하던가 해서 palette 추출을 하는식으로 정확한 rollout data를 추출하고 싶어.                                                                                                                                         
                                                                                                                                                                                                                            
- 물론 length가 길어지면 vram 메모리 이슈가 있으니, 만일 모델이 잘못 예측한건 rollout 중간에 버리던가 masking하는 식으로해서 계속 양질의 출력을 할 수 있으면 좋을 것 같아.                                                  
                                                                                                                                                                                                                            
위와 같은 접근으로 강화학습 혹은 SFT로 사용할만한 좋은 rollout 데이터를 뽑는게 가능할까?   


**지금 문제는 “VLM이 autoregressive라서 안 된다”가 핵심이라기보다, 현재 task formulation이 sequence-level RL에 너무 불리하게 잡혀 있는 것**에 가깝습니다. GSPO는 원래 token-level ratio 대신 **sequence likelihood** 기준으로 importance ratio를 두고, **sequence-level clipping / rewarding / optimization**을 하는 방식입니다. verl 문서도 GSPO를 sequence-level loss로 쓰도록 안내하고 있고, 예시 설정 역시 `loss_mode=gspo`, `loss_agg_mode=seq-mean-token-mean`, 매우 작은 clip ratio, group sampling(`rollout.n=16`)을 사용합니다. 즉, 긴 출력 전체를 하나의 trajectory처럼 다루는 성향이 강합니다.

그래서 지금 task에서는 세 가지가 동시에 터졌을 가능성이 큽니다.  
첫째, **credit assignment 붕괴**입니다. 100개 좌표에 대한 정답열을 한 번에 생성하면, 실제로 틀린 건 73번째 index 하나인데도, RL update는 긴 시퀀스 전체에 거의 같은 sign의 advantage를 밀어 넣기 쉽습니다. 특히 보상이 sequence 평균 정확도나 all-or-nothing에 가까우면, “어느 위치가 왜 틀렸는지”를 policy가 배우기 어렵습니다. 최근 후속 연구들도 sequence-level RL에서 긴 응답 길이에 대해 **고정 clipping이 short/long response를 다르게 재가중**할 수 있고, sequence를 통째로 다루면 **유효 gradient를 버리거나 underutilize**할 수 있다고 지적합니다. 이건 지금처럼 `|`로 이어진 긴 index 열에 매우 잘 맞는 실패 패턴입니다.

둘째, 당신이 짚은 **self-conditioning 문제**도 맞을 가능성이 있습니다. 다만 정확히는 “이전 출력 토큰을 본다” 자체가 문제라기보다, **이전 출력이 미래 의사결정에 본질적으로 필요하지 않은데도 prefix 안에 남아 있어서, 모델이 좌표-영상 grounding보다 label n-gram 같은 쉬운 통계를 더 활용하게 되는 것**이 문제입니다. 특히 B의 순서가 랜덤하거나 spatial locality가 약하면, autoregressive model 입장에서는 앞서 생성한 index 패턴을 따라가는 것이 더 쉬운 shortcut이 됩니다. 이건 RL에서 더 심해지기 쉽습니다. RL은 “왜 맞았는지”보다 “무슨 패턴이 reward를 줬는지”를 따라가니까요.

셋째, **task가 사실상 분류 문제인데, 이를 긴 text generation 문제로 풀고 있는 것**도 큽니다. 한 좌표당 정답은 A의 index 하나인데, 지금 formulation은 `2|3|0|...` 같은 포맷까지 모두 행동 공간에 넣고 있습니다. 그러면 모델은 “좌표에 대한 class prediction” 말고도 separator, 길이, 포맷, prefix continuation까지 같이 최적화해야 합니다. 이건 SFT보다 RL에서 훨씬 손해입니다.

그래서 제가 가장 추천하는 방법은 이겁니다.

## 1) 가장 좋은 해법: **한 번에 100개 생성하지 말고, 외부 controller가 좌표를 순회하게 바꾸기**

즉, policy가 매 step에서 **현재 좌표 하나(또는 아주 작은 chunk 4~8개)**만 보고 A의 index를 내게 하세요.  
환경 state는 `(system prompt, image1, image2, A, 현재 좌표/현재 chunk, 현재 step id)`이고, **이전 예측값들은 프롬프트에 다시 넣지 않습니다.** 이전 예측은 environment 쪽 버퍼에만 저장하고, 마지막에 모아서 최종 결과를 만들면 됩니다.

이 방식의 장점은 명확합니다.

- horizon이 100에서 1 또는 4~8로 줄어듭니다.
- 쓸데없는 self-conditioning이 사라집니다.
- reward를 좌표별 exact match로 바로 줄 수 있습니다.
- 이미 잘 된 **single-coordinate SFT**를 거의 그대로 초기 policy로 쓸 수 있습니다.

verl 쪽에서도 이 접근이 잘 맞습니다. 문서상 `RewardManager`는 **최종 token-level reward를 사용자 정의**할 수 있고, `Agent Loop` / multi-turn rollout은 **user-defined loop, environment interaction**을 지원합니다. multimodal 입력을 multi-turn rollout에 맞춰 다루는 옵션도 있습니다. 즉, “외부 controller가 step-by-step으로 좌표를 넘기고, policy가 매 step class를 내는 구조”를 충분히 만들 수 있습니다.

실제로는 이렇게 가면 됩니다.

- step t에서 B[t]만 제공
- action: A의 index 하나
- immediate reward: `1/0` exact match
- optional terminal bonus: 전 좌표 정답률이 높으면 추가 보너스
- inference 때도 동일하게 external loop로 100번 호출해서 결과를 모음

이 formulation이면, “모델이 이전에 출력한 index들을 계속 봐야 해서 spurious pattern을 학습한다”는 현재 걱정이 거의 사라집니다.


지금 문제는 “VLM이 autoregressive라서 안 된다”가 핵심이라기보다, 현재 task formulation이 sequence-level RL에 너무 불리하게 잡혀 있는 것에 가깝습니다. GSPO는 원래 token-level ratio 대신 sequence likelihood 기준으로 importance ratio를 두고, sequence-level clipping / rewarding / optimization을 하는 방식입니다. verl 문서도 GSPO를 sequence-level loss로 쓰도록 안내하고 있고, 예시 설정 역시 loss_mode=gspo, loss_agg_mode=seq-mean-token-mean, 매우 작은 clip ratio, group sampling(rollout.n=16)을 사용합니다. 즉, 긴 출력 전체를 하나의 trajectory처럼 다루는 성향이 강합니다.

그래서 지금 task에서는 세 가지가 동시에 터졌을 가능성이 큽니다.
첫째, credit assignment 붕괴입니다. 100개 좌표에 대한 정답열을 한 번에 생성하면, 실제로 틀린 건 73번째 index 하나인데도, RL update는 긴 시퀀스 전체에 거의 같은 sign의 advantage를 밀어 넣기 쉽습니다. 특히 보상이 sequence 평균 정확도나 all-or-nothing에 가까우면, “어느 위치가 왜 틀렸는지”를 policy가 배우기 어렵습니다. 최근 후속 연구들도 sequence-level RL에서 긴 응답 길이에 대해 고정 clipping이 short/long response를 다르게 재가중할 수 있고, sequence를 통째로 다루면 유효 gradient를 버리거나 underutilize할 수 있다고 지적합니다. 이건 지금처럼 |로 이어진 긴 index 열에 매우 잘 맞는 실패 패턴입니다.

둘째, 당신이 짚은 self-conditioning 문제도 맞을 가능성이 있습니다. 다만 정확히는 “이전 출력 토큰을 본다” 자체가 문제라기보다, 이전 출력이 미래 의사결정에 본질적으로 필요하지 않은데도 prefix 안에 남아 있어서, 모델이 좌표-영상 grounding보다 label n-gram 같은 쉬운 통계를 더 활용하게 되는 것이 문제입니다. 특히 B의 순서가 랜덤하거나 spatial locality가 약하면, autoregressive model 입장에서는 앞서 생성한 index 패턴을 따라가는 것이 더 쉬운 shortcut이 됩니다. 이건 RL에서 더 심해지기 쉽습니다. RL은 “왜 맞았는지”보다 “무슨 패턴이 reward를 줬는지”를 따라가니까요.

셋째, task가 사실상 분류 문제인데, 이를 긴 text generation 문제로 풀고 있는 것도 큽니다. 한 좌표당 정답은 A의 index 하나인데, 지금 formulation은 2|3|0|... 같은 포맷까지 모두 행동 공간에 넣고 있습니다. 그러면 모델은 “좌표에 대한 class prediction” 말고도 separator, 길이, 포맷, prefix continuation까지 같이 최적화해야 합니다. 이건 SFT보다 RL에서 훨씬 손해입니다.

그래서 제가 가장 추천하는 방법은 이겁니다.

1) 가장 좋은 해법: 한 번에 100개 생성하지 말고, 외부 controller가 좌표를 순회하게 바꾸기

즉, policy가 매 step에서 **현재 좌표 하나(또는 아주 작은 chunk 4~8개)**만 보고 A의 index를 내게 하세요.
환경 state는 (system prompt, image1, image2, A, 현재 좌표/현재 chunk, 현재 step id)이고, 이전 예측값들은 프롬프트에 다시 넣지 않습니다. 이전 예측은 environment 쪽 버퍼에만 저장하고, 마지막에 모아서 최종 결과를 만들면 됩니다.

이 방식의 장점은 명확합니다.

horizon이 100에서 1 또는 4~8로 줄어듭니다.
쓸데없는 self-conditioning이 사라집니다.
reward를 좌표별 exact match로 바로 줄 수 있습니다.
이미 잘 된 single-coordinate SFT를 거의 그대로 초기 policy로 쓸 수 있습니다.

verl 쪽에서도 이 접근이 잘 맞습니다. 문서상 RewardManager는 최종 token-level reward를 사용자 정의할 수 있고, Agent Loop / multi-turn rollout은 user-defined loop, environment interaction을 지원합니다. multimodal 입력을 multi-turn rollout에 맞춰 다루는 옵션도 있습니다. 즉, “외부 controller가 step-by-step으로 좌표를 넘기고, policy가 매 step class를 내는 구조”를 충분히 만들 수 있습니다.

실제로는 이렇게 가면 됩니다.

step t에서 B[t]만 제공
action: A의 index 하나
immediate reward: 1/0 exact match
optional terminal bonus: 전 좌표 정답률이 높으면 추가 보너스
inference 때도 동일하게 external loop로 100번 호출해서 결과를 모음

이 formulation이면, “모델이 이전에 출력한 index들을 계속 봐야 해서 spurious pattern을 학습한다”는 현재 걱정이 거의 사라집니다.