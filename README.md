# 강화학습 (Reinforcement learning)

![강화학습](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBSYLy%2FbtrBiHScI3y%2FBZmggRkeghDSyqomKz55H1%2Fimg.png)

<br>

Agent는 움직이는 주체이고 Environment는 학습을 통해 알고자하는 공간입니다.

시작점 S₀에서 Agent는 Environment를 모르는 상태입니다. 
Agent가 정해진 Action을 하면 Environment에서 state, reward 정보를 제공하고 얻은 정보를 바탕으로  Environment를 파악합니다.

즉 Agent는 Ation을 통해 얻는 state, reward 정보를 통해 Environment을 학습합니다.

<br>

***
 
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJUGKr%2FbtrBiroAnvL%2F9EKY4DQW6xO8PtfL6PCpRk%2Fimg.png" width=50%>

<br>

Random하게 action을 취하면 목표에 도달할 확률이 매우 낮을 것입니다. 그래서 우리는 Q 함수를 사용합니다. Q 함수는 state와 action을 입력받아서 quality(reward)를 알려줍니다. 그리고 아래 그림과 같이 q-value를 참조해서 어떤 action을 취할지 결정합니다.

<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZtEQr%2FbtrBk8alWRf%2FLhCHTAYdSaTiydR4HuLXh0%2Fimg.png" width="60%">

<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FStr3J%2FbtrBk8nSEet%2FwPISDumuQr9p2m1MIG1cuK%2Fimg.png" width="80%">

<br>

## Table of contents

<a href='#Dummy_Q_learning'>Dummy Q-learning</a><br>
<a href='#Exploit_vs_Exploration'>Exploit vs Exploration</a><br>
<a href='#References'>References</a><br>

<br>

***

<a id='Dummy_Q_learning'></a>

## Dummy Q-learning (table)
>Action을 통해 얻는 보상을 최대화 시키는 알고리즘.  

<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxDiUx%2FbtrBk8gZbk7%2FM5r1khXxz9tGVyisZLSKKk%2Fimg.png" width="60%">

<br>

> 1. Q-Table의 q-value값을 0으로 초기화합니다.
```python
# Q Table을 모두 0으로 초기화 한다. : 2차원 (number of state, action space) = (16,4)
Q = np.zeros([env.observation_space.n, env.action_space.n])

# entry_point : gym.envs 환경 불러오기
register(
    id='LakeEnv-',
    entry_point='gym.envs.toy_text:FrozenLakeEnv',
    kwargs={'map_name':'4x4', 'is_slippery':False}
)
env = gym.make('LakeEnv-')

state = env.reset() # s₀(시작점)으로 이동
```
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbIPhBi%2FbtrBlL643Vi%2FumEps7SXkjLn5l7tPRpZmk%2Fimg.png" width="40%">

<br>

> 2. 초기에는 값을 랜덤으로 주고 목표(goal)를 찾기 위해서 여러번의 action을 취합니다. 그리고 목표를 찾았을 때, q-value 값들을 갱신하고 남겨놓음으로써, 다음 에피소드를 진행할 때 목표를 잘 찾을 수 있도록 해줍니다.
```python
for i in range(num_episodes):
        state = env.reset() # state : 0,0부터 시작
        rAll = 0 # tatal reward
        done = False

        action_cnt = 0
        while not done:            
            action = rargmax(q_value[state, :]) #현재 state에서 최대 보상이 있는 action선택

            # Get new state and reward from environment
            new_state, reward, done, _ = env.step(action) # state변경

            # Update Q-Table with new knowledge using learning rate
            q_value[state, action] = reward + np.max(q_value[new_state, :]) #새로운 state에서 방금 행한 행동에 대한 정보 저장.

            state = new_state

```

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F8YVhR%2FbtrBlMETePB%2FkkiWK5nYQtiBScCkN2IKj1%2Fimg.png" width="40%">

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUDa0q%2FbtrBib0GihO%2FQrplxFBGGmkAs492iyTD8k%2Fimg.png" width="40%">

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWs654%2FbtrBk8OZL8y%2FFc3U2aS5hVVgCZTc8MWM91%2Fimg.png" width="40%">

<br>

***

<a id='Exploit_vs_Exploration'></a>

## Exploit vs Exploration

Dummy q-learning은 q-value의 최대값을 따라 움직이기 때문에 하나의 경로가 정해진 이후에 더 좋은 경로가 있어도 새로운 길을 찾는 시도를 하지않습니다. 아래와 같이 q-table로 결정된 1번 경로는 최적의 경로가 아니고 2번 경로가 최적의 경로입니다. 이와 같은 문제를 해결하기 위해서 Exploit vs Exploration 알고리즘을 사용합니다. <br>

>Exploit는 이미 알고있는 q-value값을 사용하여 action을 선택하는 것이고, Exploration은 랜덤하게 새로운 action을 선택하는 방법입니다.

<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbbAUxp%2FbtrBrsVl7AP%2FGJgPVU8nigf7uIUXg6jcik%2Fimg.png" width="50%">

<br>

그래서 우리는 E-greedy 방식을 사용하여 action을 결정합니다.
```python
e = 0.1

if rand < e:  # 랜덤하게 뽑는 값이 e보다 작으면 Exploration(탐험)하게 action
    action = random
else:         # e보다 크면 이미 알고 있는 q-value 값이 최대인 방향으로 action
    action = argmax(Q(s, a))
```

<br>

계속해서 랜덤하게 움직인다면 경로가 어느정도 정해진 이후에도 이상한 경로로 빠질 수 있습니다. <br>

그래서 학습을 거듭할수록 정해진 경로가 나오게 된 이후에도 랜덤하게 움직이는 것을 막기위해서, 학습하는 초기에는 랜덤하게 많이가고 학습 후반부에는 e값을 줄여서 탐험을 줄이고 알고 있는 값을 바탕으로 이동하도록 하는 방법인 decaying E-greedy 를 사용합니다.
```python
for i in range(episodes):
    e = 0.1 / (i + 1)
    if random(1) < e:
        action = random
    else:
        action = argmax(Q(s, a))
```
### <결과>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbV3uJt%2FbtrBsZrahb3%2FstJ6LOaG3ZoFCPXFtXwSm0%2Fimg.png" width="60%">

<br>



<a id='References'></a>

## References
[Lecture 3: Dummy Q-learning (table) - Sung Kim](https://www.youtube.com/watch?v=Vd-gmo-qO5E&list=PLlMkM4tgfjnKsCWav-Z2F-MMFRx-2gMGG&index=4)<br>
[Lab 3: Dummy Q-learning (table) - Sung Kim](https://www.youtube.com/watch?v=yOBKtGU6CG0&list=PLlMkM4tgfjnKsCWav-Z2F-MMFRx-2gMGG&index=5)<br>
[강의 슬라이드](http://hunkim.github.io/ml/)

---

[Lecture 4: Q-learning (table) exploit&exploration and discounted reward](https://www.youtube.com/watch?v=MQ-3QScrFSI&list=PLlMkM4tgfjnKsCWav-Z2F-MMFRx-2gMGG&index=6)<br>
