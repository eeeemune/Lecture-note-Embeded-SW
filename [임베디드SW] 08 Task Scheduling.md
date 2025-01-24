# [임베디드SW] 8. Task Scheduling

<aside>

# 💖 task types

</aside>

## Infinite loop

- 계속 진행되어야 하는 task.
- 보통 이러한 형태로 만들게 된다.

## finite loop

- 특정한 작업을 한 번만 수행함
- 이 경우, 작업이 끝났을 때는 **스스로를 삭제하도록 설계**하여야 한다.

```c
void MyTask(void *pdata){
    Code…
    OSTaskDel(OS_PRIO_SELF);
}
```

<aside>

**Q. task를 반드시 지워줘야 하나요?**

가용 가능한 task의 전체 개수는 한정되어 있습니다. 그래서 **RTOS에서는 특히 더이상 사용하지 않는 task를 지워줘야** 합니다.

</aside>

<aside>

# 💖 Task scheduling

</aside>

## Overview

### number of tasks

microC OS에서, 전체 task의 수는 **64개**로 한정되어 있다.

### Priority

- microC OS에서, **동일한 priority를 가지는 task는 없다.**
- priority는 `OSTaskChangePrio()` system call을 이용해 변경할 수 있다.

## Task State

![image.png](image%2011.png)

![image.png](image%2012.png)

### Running

- CPU의 개수는 결국 1개.
- 그래서 task가 아무리 많아도 **running 상태에 있는 task는 오직 1개**뿐이다.

### DORMANT

- task가 만들어져서 RAM에 있지만, **OS에는 등록되어있지 않은 경우**
- **이 경우 `ready` 상태가 아니라 `DORMANT` 상태가 된다.**

## ISR Running

interrupt service routine이 CPU를 점유하고 있는 경우

### context switching이 일어나는 경우

<aside>

**context switching** 

running 중인 task가 바뀌는 것

</aside>

1. `Task preemption`이 일어나는 경우
2. `running`상태에 있던 task가 `waiting` 상태로 넘어갈 때
3. 외부 자극에 의해 **inturrupt가 발생**했을 경우
    1. 이 경우 Interrupt Service Routine이 실행됨