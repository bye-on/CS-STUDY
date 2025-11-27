# Multilevel Feedback Queue Scheduling

<br />
<br />

# 다단계 피드백 큐 스케쥴링

- **Multilevel Queue의 방식 + 프로세스의 큐 이동**
  - Ready Queue를 여러개로 분할
    - CPU는 1개이지만, 줄을 여러개로
- 프로세스가 다른 큐로 이동 가능
- 에이징(aging)을 이와 같은 방식으로 구현할 수 있다
- Multilevel-feedback-queue scheduler를 정의하는 파라미터들
  - Queue의 수
  - 각 큐의 scheduling algorithm
  - Process를 상위 큐로 보내는 기준
  - Process를 하위 큐로 내쫓는 기준
  - 프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준

<br />

![multilevel_feedback_queue.png](./image/multilevel_feedback_queue.png)

- 다단계 피드백 큐 스케줄링 알고리즘에서는 프로세스가 큐들 사이를 이동하는 것을 허용

**동작 예시**

- Three Queues
  - Q0 - time quantum 8 milliseconds
  - Q1 - time quantum 16 milliseconds
  - Q2 - FCFS
- Scheduling
  - new job이 queue Q0로 들어감
  - CPU를 잡아서 할당 시간 8 milliseconds 동안 수행됨
  - 8 milliseconds 동안 다 끝내지 못했으면 queue Q1으로 내려감
  - Q1에 줄서서기다렸다가 CPU를 잡아서 16 ms 동안 수행됨
  - 16 ms에 끝내지 못한 경우 queue Q2로 쫓겨남
