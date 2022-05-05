# 강화학습 (Reinforcement learning)

![강화학습](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBSYLy%2FbtrBiHScI3y%2FBZmggRkeghDSyqomKz55H1%2Fimg.png)


Agent는 움직이는 주체이고 Environment는 학습을 통해 알고자하는 공간입니다.

시작점 S₀에서 Agent는 Environment를 모르는 상태입니다. 
Agent가 정해진 Action을 하면 Environment에서 state, reward 정보를 제공하고 얻은 정보를 바탕으로  Environment를 파악합니다.

즉 Agent는 Ation을 통해 얻는 state, reward 정보를 통해 Environment을 학습합니다.

## Q-value

<center><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJUGKr%2FbtrBiroAnvL%2F9EKY4DQW6xO8PtfL6PCpRk%2Fimg.png" width=60%></center>

<br>

## Table of contents

<a href='#Dummy_Q_learning'>Dummy Q-learning</a><br>
<a href='#Tutorials'>Tutorials</a><br>
<a href='#Multi-Armed Bandits'>Multi-Armed Bandits</a><br>

<br>

***

<a id='Dummy_Q_learning'></a>

## Dummy Q-learning (table)
>Action을 통해 얻는 보상을 최대화 시키는 알고리즘.  

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxDiUx%2FbtrBk8gZbk7%2FM5r1khXxz9tGVyisZLSKKk%2Fimg.png" width="80%">

<br>

1. Q-Table의 q-value값을 0으로 초기화합니다.



