# **임베디드소프트웨어(CSE3304) 전범위 강의 필기노트**

2024 인하대학교 컴퓨터공학과 전공수업

## **목차**

### [**1. 임베디드 시스템이란?**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2001%20%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3%20%E1%84%89%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%A6%E1%86%B7%E1%84%8B%E1%85%B5%E1%84%85%E1%85%A1%E1%86%AB.md)

- 💖 임베디드 시스템이란?
    - 마이크로 프로세서 vs 마이크로 컨트롤러
        - 마이크로 프로세서
        - 마이크로 컨트롤러
- 임베디드 시스템의 활용 분야
- 임베디드 시스템 구성
    - X-platfrom과 X-compiler(크로스 컴파일러)

### [**2. Real-time Embeded System**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2002%20Real-time%20Embeded%20System.md)

- 💖 실시간 임베디드 시스템
    - Real-time system의 정의
    - Real-time system의 특징
        - Timeliness
        - Predictablity
    - 실시간 시스템의 분류
        - Hard real-time system 의 단점

### [**3. Embeded 운영체제**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2003%20Embeded%20%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8E%E1%85%A6%E1%84%8C%E1%85%A6.md)

- 💖 임베디드 운영체제
    - 임베디드 SW의 역사
        - 초기의 임베디드 시스템
        - 시기 별 코드 길이의 변화
        - 임베디드 시스템 전용 OS의 등장
    - 널리 사용되는 임베디드 OS
        - RTOS
        - 임베디드 리눅스
        - 기타
- 💖 μC/OS-II
    - micro C의 특징

### [**4. 선점형 커널과 비선점형 커널**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2004%20%E1%84%89%E1%85%A5%E1%86%AB%E1%84%8C%E1%85%A5%E1%86%B7%E1%84%92%E1%85%A7%E1%86%BC%20%E1%84%8F%E1%85%A5%E1%84%82%E1%85%A5%E1%86%AF%E1%84%80%E1%85%AA%20%E1%84%87%E1%85%B5%E1%84%89%E1%85%A5%E1%86%AB%E1%84%8C%E1%85%A5%E1%86%B7%E1%84%92%E1%85%A7%E1%86%BC%20%E1%84%8F%E1%85%A5%EB%84%90.md)

- 💖 OS: 선점형 커널과 비선점형 커널
    - 비선점형 커널
        - 비선점형 커널에서 ISR의 작동 방식
    - 선점형 커널
    - 비선점형 커널 vs 선점형 커널

### [**5. Mutual Exclusion**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2005%20Mutual%20Exclusion.md)

- 💖 OS: Mutual exclusion
    - 어떻게 Mutual exclusion을 구현할 수 있나요?
        - 인터럽트 발생을 방지
        - Semaphore
    - Semaphore
        - 세마포어 은닉
        - Deadlock(교착상태)

### [**6. Inter-Task Communication (ITC)**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2006%20Inter-Task%20Communication%20(ITC).md)

- 💖 OS: 태스크 간 통신
    - 태스크끼리 어떻게 정보를 주고 받을 수 있나요?
        - Global variable
        - message passing

### [**7. Reentrancy Code**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2007%20Reentrancy%20Code.md)

- 💖 OS: Reentrancy code
    - 함수를 Reentrantive하게 만들려면 어떻게 해야 하나요?
    - 재진입 불가능 함수는 왜 문제가 생기나요?
        - 재진입 가능 함수
        - 재진입 불가능 함수

### [**8. Task Scheduling**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2008%20Task%20Scheduling.md)

- 💖 task types
    - Infinite loop
    - finite loop
- 💖 Task scheduling
    - Overview
        - number of tasks
        - Priority
    - Task State
        - Running
        - DORMANT
    - ISR Running
        - context switching이 일어나는 경우

### [**9. Task Control Block (TCB)**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2009%20Task%20Control%20Block%20(TCB).md)

- 💖 Task Control Block
    - TCB의 내부 포인터
        - ⚡ OSTCBStkPtr
        - ⚡ OSTCBStkBottom
        - ⚡ OSTCBStkSize
        - ⚡ OSTCBDly
        - ⚡ OSTCBStat
    - TCB들의 관리
        - Free list
        - OSTBList
        - 예시: 11개의 task를 생성한다면?

### [**10. Ready List**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2010%20Ready%20List.md)

- 💖 Ready list
    - Data structure
        - ⚡ OSRdyGrp
        - ⚡ OSRdyTbl[]
        - ⚡ OSMapTbl[]
        - ⚡ OSUnMapTbl[]
    - Priority position
        - 예시 1. OSRdyTbl로 OSRdyGrp 채우기
        - 예시 2. 17번 task가 새로 ready 상태가 되었다면?
        - 예시 3. 17번 task가 wait 상태로 변했다면?
    - Find highest proirity task

### [**11. OS Scheduling**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2011%20OS%20Scheduling.md)

- 💖 OS Scheduling
    - OS Switching 구현 방법
    - Task 수준에서의 context switching
        - 초기 상황
        - Context Switching이 일어날 때

### [**12. ISR Scheduling**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2012%20ISR%20Scheduling.md)

- 💖 ISR Scheduling
    - ISR이란?
        - ISR의 진행 과정
    - 동시에 여러 interrupt가 발생했을 때 scheduling하는 방법
    - ISR Scheduling

### [**13. Clock Ticks**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2013%20Clock%20Ticks.md)

- 💖 Clock ticks
- uC/OS-II에서의 tick
    - Tick ISR

### [**14. Idle Task in Micro-C OS**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2014%20Idle%20Task%20in%20Micro-C%20OS.md)

- 💖 Idle Task
    - Idle Task
    - Statistics Task
        - Statistics Task가 CPU usage를 계산하는 방법

### [**15. Event Control Block (ECB)**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2015%20Event%20Control%20Block%20(ECB).md)

- 💖 OS: Event control block
    - Event control block이란 무엇인가요?
    - Event Control Block의 역할
    - ECB의 구조
        - OSEventType
        - OSEVENTCnt
        - OSEVENTPtr
        - OSEventGrp, OSEventTbl[]
        - bits
    - ECB가 우선순위를 scheduling하는 방법
        - Insert Task to event wait list
        - Remove Task From event wait list
    - Free Event Control Block List
    - ECB Operations
        - ⚡ OS_EventWaitListInit(OS_EVENT *pevent)
        - ⚡ OS_EventTaskRdy()
        - ⚡ OS_EventWait()
        - ⚡ OS_EventTO()
    - MicroC implements
        - OS_EventTaskRdy()
        - OS_EventTaskWait()
        - OS_EventTO

### [**16. Mailbox**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2016%20Mailbox.md)

- 💖 Mailbox
    - Operations
        - mailbox 생성
        - mailbox 삭제
        - mailbox에서 메시지 기다리기
- if OS_ARG_CHK_EN > 0
- endif
- mailbox에 message 보내기
- Mailbox 동작 예시
    - Msg가 있을 경우
    - Msg가 없을 경우
- OSMboxPostOpt()
- Mailbox의 한계

### [**17. Message Queue**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2017%20Message%20Queue.md)

- 💖 Message Queue
    - Queue control block
    - Free list
    - Circular buffer
    - Operations
        - Message queue 생성
- define MSG_QUEUE_SIZE 20

### [**18. Counting Semaphore**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2018%20Counting%20Semaphore.md)

- 💖 Counting Semaphore
    - Opreations
        - ⚡ OSSemCreate()
        - ⚡ OSSemDel()
        - ⚡ OSSemPost()
        - ⚡ OSSemAccept()
        - ⚡ OSSemQuery()
    - 세마포어 생성
        - OSSemCreate()가 return되기 직전의 ECB
    - 세마포어 대기
        - timeout
        - Semaphore
    - 세마포어 대기

### [**19. Event Flag**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2019%20Event%20Flag.md)

- 💖 Event Flag
    - Event flag들의 조합
    - event flag를 이용한 태스크 동기화 예시
        - 우선 순위 높은 태스크를 우선 순위 낮은 2개의 태스크가 특정 시점까지 블록킹하는 경우
        - 우선 순위 낮은 2개의 태스크를 우선 순위 높은 태스크가 특정 시점까지 블록킹하는 경우
    - Event flag group structure
        - Event flag group
        - Event Flags Group Node Structure
    - Operations
        - ⚡ OSFlagCreate
        - ⚡ OSFlagDel
        - ⚡ OSFlagPend
        - ⚡ OSFlagCreate
        - ⚡ OSFlagCreate
        - ⚡ OSFlagCreate
    - Flag group을 이용한 task control의 과정…
        - OS_Flag_GRP생성
        - 해당 Flag group을 사용하는 task가 생성됨
        - task가 여러개 생김 → node들이 doubly linked list로 연결
        - 이 때 OS_FLAG_GRP의 FLAGS들에 변화가 생겼다면?

### [**20. RTOS**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2020%20RTOS.md)

- 💖 실시간 스케줄링
    - 임베디드에서의 스케줄링
    - 실시간 스케줄링의 종류
    - RMS, EDF의 전제

### [**21. Rate Monotonic Scheduling (RMS)**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2021%20Rate%20Monotonic%20Scheduling%20(RMS).md)

- 💖 Rate Monotonic Scheduling(RMS)
    - RMS의 도입 배경
        - 예시
    - RMS이란?
    - Feasiblity
    - CPU Utilization
        - RMS의 CPU Utilization과 feasiblity
        - 주의) Utilization 식은 필요충분조건이 아니라 필요조건이다
    - RMS 방식의 문제점
        - 예시 상황

### [**22. Earliest Deadline First (EDF)**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2022%20Earliest%20Deadline%20First%20(EDF).md)

- 💖 Earliest Deadline First- EDF
    - EDF 방식
    - EDF 방식의 장단점
        - 장점
        - 단점

### [**23. 우선순위 계승 프로토콜**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2023%20%E1%84%8B%E1%85%AE%E1%84%89%E1%85%A5%E1%86%AB%E1%84%89%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%B1%20%E1%84%80%E1%85%A8%E1%84%89%E1%85%B3%E1%86%BC%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%90%E1%85%A9%E1%84%8F%E1%85%A9%E1%86%AF.md)

- 💖 실시간 스케줄링의 문제점과 해결책
- 💖 우선 순위 전도 현상
    - 우선 순위 전도 현상이란?
    - 언제 이런 상황이 발생하나요?
        - 발생 상황 예시
- 💖 우선 순위 계승 프로토콜
    - 우선 순위 전도 현상의 해결 방법
    - 우선 순위 계승 프로토콜이란?
    - 우선 순위 계승 프로토콜에서 발생할 수 있는 문제점
        - push-through blocking
    - 우선 순위 계승 프로토콜로도 해결이 안 되는 문제점
        - transitive blocking
        - Deadlock

### [**24. 우선순위 상한 프로토콜**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2024%20%E1%84%8B%E1%85%AE%E1%84%89%E1%85%A5%E1%86%AB%E1%84%89%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%B1%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%90%E1%85%A9%E1%84%8F%E1%85%A9%E1%86%AF.md)

- 💖 우선순위 상한 프로토콜
    - 아이디어
        - 우선순위 상한 프로토콜의 장점
        - 자원의 우선 순위는 어떻게 결정하나요?
    - 우선순위 상한 프로토콜을 사용하는 예시

### [**25. ATmega128 board**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2025%20ATmega128%20board.md)

- 💖 ATmega128 보드
    - 보드 구성도
    - ATmega 128 Board 설명
    - 메모리
    - Resistor Setting
        - Pull Up Resistor
        - Pull Down Resistor
- 💖 ATmega128 임베디드 프로그래밍
    - 시간 지연 시스템 제공 함수
- define F_CPU 16000000UL
- include <util/delay.h>

### [**26. Volatile Keyword**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2026%20Volatile%20Keyword.md)

- Volatile keyword
    - Sample 1
    - Sample 2
    - 어떤 상황에 필요한가요?

### [**27. LED와 FND 제어**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2027%20LED%E1%84%8B%E1%85%AA%20FND%20%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5.md)

- 💖 LED 제어
    - ATmega128 보드의 LED
        - LED 작동 원리
    - FND

### [**28. Metux Semaphore**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2028%20Metux%20Semaphore.md)

- 💖 Mutex Semaphore
    - MicroC OSII에서의 우선순위 계승 프로토콜
        - 예시
- 💖 uCOS에서의 Mutex Semaphore 구현
    - Mutex Creation
        - 사용
        - 구현
    - Mutex Pend
        - Mutex Deletion

### [**29. I/O 장치 통신**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2029%20I%20O%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8E%E1%85%B5%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB.md)

- 💖 Precessor와 장치 간 Communication
    - BUS
    - Processor와 주변 장치의 통신
        - ⚡ Port-based I/O
        - ⚡ Bus-based I/O
    - Bus-based I/O
        - ⚡ Memory-mapped I/O
        - ⚡ I/O mapped I/O(Standard I/O)

### [**30. Interrupt**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2030%20Interrupt.md)

- 💖 인터럽트
    - interrupt 발생 시 대응 시나리오
- 💖 Atmega 128 에서의 GPIO
    - GPIO

### [**31. Performance Calculating**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2031%20Performance%20Calculating.md)

- 💖 Performance
    - Background
    - CPU execution time for a program
    - CPI
    - MIPS
        - Mflops, Gflops, Petaplops

### [**32. Switch와 Interrupt**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2032%20Switch%E1%84%8B%E1%85%AA%20Interrupt.md)

- 💖 스위치를 활용한 interrupt
    - 임베디드 시스템에서 외부 입력을 확인하는 방법
        - while문을 이용해서 계속 확인하기
        - interrupt 사용
- interrupt 발생을 알아채는 trigger
    - ⚡️ edge trigger
    - ⚡️ level trigger
- 💖 ATmega128에서의 Interrupt 활용
    - Register setting
        - ⚡️ EIMSK
        - ⚡️ EICR
        - ⚡️ ISR 등록

### [**33. Buzzer와 Frequncy**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2033%20Buzzer%E1%84%8B%E1%85%AA%20Frequncy.md)

- 💖 Buzzer
    - Buzzer란?
    - ATmega128에서의 buzzer
        - 예시: 단순히 buzzer를 울리는 프로그램
- include <avr/io.h>
- define F_CPU 16000000 UL-include <util/delay.h>
- 주파수에 따른 음계 출력하기
    - 주파수에 따른 음계
    - ‘도’ 출력하기

### [**34. Timer와 Prescaler**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2034%20Timer%E1%84%8B%E1%85%AA%20Prescaler.md)

- 💖 Timer
    - 8비트 타이머
        - ATmega128에서의 카운터(타이머)
    - Prescaler
        - Prescaler의 활용: 예시
        - Prescaler의 등장 배경
    - 8비트 타이머관련 레지스터
        - ⚡ TCCRn
        - ⚡ TCCTn
        - ⚡ TIMSK
        - ⚡ TCNTn
    - 예제: 타이머로 원하는 시간을 세팅하기
        - 1clock에 몇 초 걸리는지?
        - 100us만큼의 시간이 되려면 몇 clock이 필요한지?
        - TCNT에 원하는 값 저장
- 💖 예제 프로그램
    - ‘도’ 음 내기

### [**35. CdS Sensor와 A/D Converter**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2035%20CdS%20Sensor%E1%84%8B%E1%85%AA%20A%20D%20Converter.md)

- 💖 광센서
    - CdS 센서
    - A/D 컨버터
        - Resolution
        - conversion time
    - ATmega128 A/D 컨버터 레지스터
        - ⚡ ADMUX
        - ⚡ ADCSRA
        - ⚡ ADCH, ADCL

### [**36. ARM Architecture**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2036%20ARM%20Architecture.md)

- 💖 ARM Architecture
    - ISA
    - ARM
        - ARM Architecture의 특징
        - RISC Architecture
    - System-on-chip
    - ARM architecture의 특징
        - ⚡ 간단한 하드웨어
        - ⚡ Load/store architecture
        - ⚡ Fixed-length instruction fields
    - Endianess
        - ⚡ Little-endian memory system
        - ⚡ Big-endian memory system
        - The effect of endianness

### [**37. Pipelining**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2037%20Pipelining.md)

- 💖 Pipelining
    - Background: Patterson’s Laundry
        - pipelining의 필요성
    - Pipeling이 사용되는 instruction들
        - ⚡ Fetch
        - ⚡ Decode
    - Instruction time의 길이와 pipelining
        - Cortex-A7
        - Cortex-A15
        - instruction을 쪼개서 pipelining이 길어질 수록 좋은가요?
- 💖 ARM ISA

### [**38. ARM ISA**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2038%20ARM%20ISA.md)

- 💖 ARM ISA
    - Condition bit
        - CPSR
    - ⚡ Data processing instructions
        - Data processing instruction의 종류
        - Standard format
        - Instruction encoding
        - 예시
    - ⚡ Branch instructions
        - Standard format
        - unconditional branch
        - Instruction encoding
        - 예시
    - ⚡ Status register transfer instructions
        - MSR과 MRS 명령어
        - CPSR
        - Example
    - ⚡ Load and store instruction
        - LDR / STR
        - LDM / STM
        - SWP

### [**39. Load and Store Instructions**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2039%20Load%20and%20Store%20Instructions.md)

- 💖 Load and store instructions
    - ⚡ Single register data transfer
    - ⚡ Block register data transfer
        - 사용 예시
- 💖 Address modes
    - Address modes
        - ⚡ LDMIA
        - ⚡ LDMDA
        - ⚡ LDMIB
    - Stack operations
        - ⚡ Full stack
        - ⚡ Empty Stack
        - ⚡ Descending stack
        - ⚡ Ascending stack
        - 예시: Full Descending stack

### [**40. Exception Generating Instruction**](https://github.com/eeeemune/Lecture-note-Embeded-SW/blob/main/%5B%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%87%E1%85%A6%E1%84%83%E1%85%B5%E1%84%83%E1%85%B3SW%5D%2040%20Exception%20Generating%20Instruction.md)

- 💖 Exception-generating instruction
    - Format
    - Processor modes
        - ⚡ User mode
        - ⚡ FIQ mode
        - ⚡ IRQ mode
        - ⚡ Supervisor mode
        - ⚡ Abort mode
        - ⚡ Undefined mode
    - Registers
        - ⚡ General purpose Registers
        - ⚡ Status Registers
