# [임베디드SW] 13. Clock Ticks

<aside>

# 💖 Clock ticks

</aside>

- 시간 기반의 interrupt의 일종
- 일정한 시간이 정해져 있는 게 아니라, 시간의 단위같은 느낌이라고 하심
    - 몇초일지는 OS마다 다르다

### uC/OS-II에서의 tick

```cpp
void OSTickISR(void) {
	Save processor registers;
	Call OSIntEnter()
		Call OSTimeTick();
	Call OSIntExit();
	Restore processor registers;
	Execute a return from interrupt instruction;
}
```

## Tick ISR

```cpp
void OSTimeTick(void)
{
	while (ptcb->OSTCBPrio != OS_IDLE_PRIO) {
		OS_ENTER_CRITICAL();
		if (ptcb->OSTCBDly != 0) {
			if (--ptcb->OSTCBDly == 0) {
				if (!(ptcb->OSTCBStat & OS_STAT_SUSPEND)) {
					OSRdyGrp |= ptcb->OSTCBBitY;
					OSRdyTbl[ptcb->OSTCBY] |= ptcb->OSTCBBitX;
				}
				else {
					ptcb->OSTCBDly = 1;
				}
			}
		}
		ptcb = ptcb->OSTCBNext;
		OS_EXIT_CRITICAL();
	}
}
```

- while (ptcb->OSTCBPrio != OS_IDLE_PRIO)
    - `OS_IDLE_PRIO ==  OSTCBPrio`라면 **wait하고 있는 task가 하나도 없다는 의미**이다.
        - OS_IDEL_PRIO는 가장 우선순위가 낮은 task인데, 그게 가장 우선순위가 높은 task인 상황이라면 기다리고 있는 task가 하나도 없다는 의미
- if (--ptcb->OSTCBDly == 0)
    - tick이 하나 지나갔다는 것을 알림
- OSTCBDly == 0이 되었을 때,
    - OSTCBStat
        - 유저가 만든 게 아니라 micro-C OS에서 자체적으로 사용함
    - OS_STAT_SUSPEND
        - 이 task가 suspend 상태인지 확인
    - OSRdyGrp |= ptcb->OSTCBBitY;
        - ready group으로 옮김