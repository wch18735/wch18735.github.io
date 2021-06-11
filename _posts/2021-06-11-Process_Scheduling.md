---
title: "[OS] 프로세스 스케쥴링(2)"
excerpt: "process scheduling"
date: 2021-06-10
layout: single
classes: wide
category:
    - os
tag:
    - operating system
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

### Scheduler Diagram

[여기](https://velog.io/@ss-won/OS-CPU-Scheduler%EC%99%80-Dispatcher)에 깔끔하게 설명된 그림이 하나 더 있습니다. 출처에 보시면 프로세스 상태 변화가 언제 일어나는지 등을 확인할 수 있습니다.

![scheduler diagram](/_img/2021-06-11/scheduler_diagram.jpg){: .align-center}

### CPU Scheduling Criteria (성능 척도)

CPU Scheduling 알고리즘을 알아보기 전, 각 알고리즘들이 어떤 기준으로 평가되는지를 확인하겠습니다. CPU 스케줄링의 성능 척도는 아래와 같이 다섯 가지가 있습니다.

|성능 척도|특징|
|:---:|---|
|프로세서 이용률<br>(CPU Utilization)|**시간당 CPU를 사용한 시간의 비율**<br>프로세서를 실행 상태로 항상 유지하여 유휴상태가 되지 않도록 함<br>I/O 중심 작업보다 프로세서 중심 작업을 실행해야 함|
|처리율<br>(Throughput)|**시간당 처리한 시간의 비율**<br>단위 시간당 완료되는 작업 수가 많도록 정책 수립<br>짧은 작업을 우선 처리하거나 인터럽트 없이 작업을 실행하는 것을 지향|
|반환시간<br>(Turnaround Time)|**CPU burst time (쓰고 나갈 때까지 시간)**<br>아래 시간들의 합으로 구성<br>&ensp;ㆍ작업이 시스템에 맡겨져서 메인 메모리에 들어가기까지 시간<br>&ensp;ㆍ준비 큐에 있는 시간<br>&ensp;ㆍ실행 시간<br>&ensp;ㆍ입출력 시간<br>&ensp;ㆍ완료 시간|
|대기시간<br>(Waiting Time)|**대기열에 들어와 CPU를 할당받기까지 걸린 시간**<br>Ready 상태로 총 얼마나 대기하였는가로 계산|
|응답시간<br>(Response Time)|**대기열에서 처음으로 CPU를 얻을 때까지 걸린 시간**<br>Ready에서 Running으로 넘어갈 때까지 걸린 시간<br>대화형 프로그램에서는 응답시간이 빠른 것이 중요함|


### Types of CPU Scheduling Algorithm (CPU 스케줄링 알고리즘 종류)

주요한 CPU 스케줄링 알고리즘은 아래와 같이 6개가 있습니다.

1. First Come First Serve (FCFS)
2. Shortest-Job-First (SJF) Scheduling
3. Shortest Remaining Time
4. Priority Scheduling
5. Round Robin Scheduling
6. Multilevel Queue Scheduling

![scheduling algorithms](/_img/2021-06-11/scheduling_algorithms.jpg){: .align-center}

#### First Come First Serve

가장 간단한 스케줄링 방법입니다. 먼저 CPU를 요청하는 프로세스에 CPU 자원이 할당됩니다. **FIFO Queue**를 이용해 구현될 수 있으며 관리 가능합니다. 프로세스가 **Ready Queue**에 들어갈 때, **PCB(Process Control Block)** 이 큐의 끝 부분에 link 되고 작업이 끝나면 시작 주소를 다음 PCB로 넘겨줍니다.

알고리즘의 특징은 다음과 같습니다.

- 비선점형, 선점형 모두 가능
- 먼저 요청하는 순서대로 CPU 자원을 할당받음
- 구현과 사용이 용이함
- 낮은 성능을 보여주며 대기시간이 굉장히 길어지는 단점이 있음

#### Shortest Remaining Time

이 알고리즘은 **SJF preemptive scheduling** 기법으로도 많이 알려져 있습니다. 선점형 최소 작업 우선 알고리즘은 **Ready Queue**에 있는 프로세스의 작업이 더 빠르다면 현재 Running 프로세스를 멈추고 CPU를 강제로 점유할 수 있음을 뜻합니다.

알고리즘의 특징은 다음과 같습니다.

- SJF 기반 배치 환경에서 많이 적용됨
- 프로세스별 정확한 CPU 사용시간을 알 수 없어 실제로 구현되기 어려운 알고리즘
- 만약 모든 프로세스 동작 시간을 알 수 있다면, **Ready Queue**를 Heap 구조로 구현하여 가장 빨리 끝나는 프로세스를 찾을 수 있음
- **기아(Starvation) 현상**이 발생할 수 있는데, 이는 **대기시간에 비례하는 가중치 부여**를 통해 해결할 수 있음

#### Priority Based Scheduling

프로세스에 부여된 우선순위를 기반으로 동작하는 알고리즘입니다. 선점형과 비선점형 모두 가능합니다. 이 알고리즘 역시 기아 현상이 발생할 수 있습니다. 이 문제는 대기열에 오래 있는 Process들의 우선순위를 높이는 **Aging** 기법을 통해 해결할 수 있습니다. 우선순위는 프로세스가 요구하는 메모리, 시간 등의 다양한 요소에 따라 결정될 수 있습니다.

#### Round-Robin Scheduling




### References

- [guru99.com](https://www.guru99.com/cpu-scheduling-algorithms.html)
- [GeeksforGeeks](https://www.geeksforgeeks.org/cpu-scheduling-in-operating-systems/)
