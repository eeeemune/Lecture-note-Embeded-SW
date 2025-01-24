# [임베디드SW] 11. OS Scheduling

<aside>

# 💖 OS Scheduling

</aside>

## OS Switching 구현 방법

```cpp
void OSSched(void) {
	INT8U y;
	OS_ENTER_CRITICAL();
	if ((OSLockNesting | OSIntNesting) == 0) {
		y = OSUnMapTbl[OSRdyGrp];
		OSPrioHighRdy = (INT8U)((y << 3) + OSUnMapTbl[OSRdyTbl[y]]);
		if (OSPrioHighRdy != OSPrioCur) {
			OSTCBHighRdy = OSTCBPrioTbl[OSPrioHighRdy];
			OSCtxSwCtr++;
			OS_TASK_SW();
		}
	}
	OS_EXIT_CRITICAL();
}
```

- semaphore로 critical section을 보호
- if ((`OSLockNesting` | `OSIntNesting`) == 0)
    - 실수로 들어왔더라도 보호해줌
- `y` = `OSUnMapTbl`[`OSRdyGrp`];
    - y에 우선순위가 가장 높은 group을 할당
- `OSPrioHighRdy` = (`INT8U`)((`y` << 3) + `OSUnMapTbl`[`OSRdyTbl`[`y`]]);
    - 우선순위가 가장 높은 task를  `OSPrioHighRdy`에  할당
- if (`OSPrioHighRdy` != `OSPrioCur`)
    - 만약 가장 운선순위가 높은 task를 현재 수행중이었다면 그냥 pass
- **OSTCBHighRdy = OSTCBPrioTbl[OSPrioHighRdy];**
    - 이게 핵심 코드인 것 같은데, 말씀하시는 걸 못 받아 적었다…
    - 실행할 h

## Task 수준에서의 context switching

```cpp
//Task 수준에서의 context switch
void OSCtxSW(void)
{
	Values of R1, R2, R3, R4 and so on are pushed to stack;
	OSTCBCur->OSTCBStkPtr = SP;
	OSTCBCur = OSTCBHighRdy;
	SP = OSCBHighRdy->OSTCBStkPtr;
	Values of R4, R3, R2, R1 and so on are popped;
}
```

### 초기 상황

![image.png](image%2021.png)

- 왼쪽의 Low-priority task가 현재 실행 중인 process의 TCB
- 오른쪽의 High-priority task가 끼어 들어야 함
- CPU Stack에는 `SP` pointer가 있다
    - **이 SP pointer는 현재 실행 중인 TCB의 위치를 가리키고 있다.**
- 

### Context Switching이 일어날 때

![image.png](image%2022.png)

- SP를 `OSTCBCur`에 저장
    - stack pointer(`SP`)는 현재 진행 중이었던 stack의 section을 가리키고 있음
- `run` 상태이던 TCB를 `ready` 상태로 바꿈

![image.png](image%2023.png)

- `OSTCBHighRdy`에는 그 PCB의 마지막 상태를 담은 SP가 있을 것
- 그 값을 다시 CPU의 `SP`에 옮겨 온다
- 그 다음 TCB를 쭉 실행