# Multilevel Queue Scheduling

<br />
<br />

# 다단계 큐 스케쥴링

- Ready Queue를 여러개로 분할
  - CPU는 1개이지만, 줄을 여러개로
  - Ready 큐를 여러 개 만들어 각각에 대해 다른 우선순위와 스케줄링 알고리즘을 사용하는 기법
- 각 큐는 독립적인 스케줄링 알고리즘을 가짐
  - foreground Queue ( Interactive )
    - Interactive한 동작이 필요한 프로세스를 위한 큐
    - 큐 동작 : RR (Round Robin) 기법 사용
  - background Queue ( Batch - No Human Interaction )
    - CPU 연산 작업을 주로 수행하는 프로세스를 위한 큐
    - 큐 동작 : FCFS
- 큐에 대한 스케줄링이 필요 (우선순위도 존재)
  - **Fixed priority scheduling**
    - serve all from foreground then from background.
    - foreground queue 작업을 우선 수행( foreground queue 우선순위를 둠 )
    - 기아 상태 발생 가능성 존재
  - **Time slice**
    - 각 큐에 CPU Time을 적절한 비율로 할당
    - 예) 80% to foreground in RR, 20% to background in FCFS

<br />

![multi_level_queue.png](./image/multi_level_queue.png)

- 프로세스들이 시스템 진입 시에 영구적으로 하나의 큐에 할당
