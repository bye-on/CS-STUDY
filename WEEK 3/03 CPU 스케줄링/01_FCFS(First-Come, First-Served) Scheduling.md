# FCFS(First-Come, First-Served) Scheduling

<br />

# 선입 선처리 스케쥴링

![fcfs.png](<image/FCFS(First-Come,%20First-Served)%20Scheduling/fcfs.png>)

- **Non-preemptive**
- CPU를 먼저 요청하는 프로세스가 CPU를 먼저 할당받는 방식 (먼저 도착한 순서대로 처리)
- FIFO(First in, First out) Queue로 쉽게 관리, 구현 가능
- 단점은, 평균 대기 시간이 때에 따라 매우 길어질 수 있다는 점

<br />
<br />

![fcfs_switch.png](<image/FCFS(First-Come,%20First-Served)%20Scheduling/fcfs_switch.png>)

- 평균 대기 시간은 일반적으로 최소가 아니며, 프로세스 CPU burst time이 크게 변할 경우에는 평균 대기 시간도 크게 변할 수 있다.

<br />

## Convoy Effect ( 호위 효과 )

- 모든 다른 프로세스들이 하나의 긴 프로세스가 CPU를 양도하기를 기다리는 것
- 짧은 프로세스들이 먼저 처리되도록 허용될 때보다 CPU와 장치 이용률이 저하되는 결과를 발생시킴
