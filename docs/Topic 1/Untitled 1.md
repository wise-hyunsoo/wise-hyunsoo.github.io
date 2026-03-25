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