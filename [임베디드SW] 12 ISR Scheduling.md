# [임베디드SW] 12. ISR Scheduling

<aside>

# 💖 ISR Scheduling

</aside>

- μC/OS-II는 `ISR(interrupt service routine)`이 assembly code로 짜여 있음.

## ISR이란?

<aside>

**ISR** 

Interrupt Service Routine. CPU가 interrupt driven으로 여러 task들을 수행하는 방식

</aside>

### ISR의 진행 과정

![image.png](image%2024.png)

- 각 interrupt들은 CPU 안에서 각자 자신들의 register를 가진다.
    - 우선, interrupt는 다이렉트로 CPU에 꽂힌다.
    - 이 때, 키보드 입력 등의 모든 interrupt를 전부 interrupt pin으로 만들기는 어렵기 때문에 각 interrupt들은 CPU 안에서 각자 자신들의 register를 가진다.
- 예를 들어, 마우스 클릭이라는 interrupt가 발생했다고 하자.
    - CPU는 자신의 schedule대로 task들을 수행하고 있는 상황
    - interrupt가 발생하면 해당 사실을 CPU에 알림
    - 그럼 CPU는 일단 task를 중단함
    - 그리고 현재 Context를 저장해 둠
    - 그 다음 register들을 전부 확인해서 어떤 interrupt가 자신에게 signal을 보냈는지 확인함
        - 3번 register의 값이 1이라면 3번 interrupt가 발생헀구나 하는 식…
        - 이를 `vectoring`이라고 함
    - ISR이 종료되었을 때, CPU는 하던 일로 돌아간다.
        - 미리 저장해 둔 context로 돌아감

## 동시에 여러 interrupt가 발생했을 때 scheduling하는 방법

- 우선, 하나의 interrupt register가 1이 되면 자신의 ISR이 시작되기 전까지 다른 interrupt들이 disabled된다.
    - **CPU가 이 inturrupt가 3번 interrupt구나! 알게 된 후 다시 register들이 abled된다.**

## ISR Scheduling

```cpp
void OSIntExit(void)
{
	OS_ENTER_CRITICAL();
	if ((--OSIntNesting | OSLockNesting) == 0) {
		OSIntExitY = OSUnMapTbl[OSRdyGrp];
		OSPrioHighRdy = (INT8U)((OSIntExitY << 3) +
			OSUnMapTbl[OSRdyTbl[OSIntExitY]]);
		if (OSPrioHighRdy != OSPrioCur) {
			OSTCBHighRdy = OSTCBPrioTbl[OSPrioHighRdy];
			OSCtxSwCtr++;
			OSIntCtxSw();
		}
	}
	OS_EXIT_CRITICAL();
}
```

- 생각해보면, 해당 함수가 호출될 때 원래 CPU에서 돌아가던 task는 **이미 자신의 TCB에 context를 저장해 둔 상태**이다.
    - 거기서 task scheduling과의 차이점이 발생하는 거라고 말씀하심
- 따라서 이전 task의 context를 CPU stack에 저장할 필요가 없음