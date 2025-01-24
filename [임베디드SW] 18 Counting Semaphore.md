# [임베디드SW] 18. Counting Semaphore

<aside>

# 💖 Counting Semaphore

</aside>

## Opreations

### ⚡ OSSemCreate()

세마포어 생성

### ⚡ OSSemDel()

세마포어 삭제

### ⚡ OSSemPost()

세마포어에 이벤트 발생을 알림(signal). 세마포어를 기다리는 태스크 중 가장 우선 순위가 높은 태스크가 실행됨.

### ⚡ OSSemAccept()

세마포어 획득. **단, 세마포어가 사용가능하지 않더라도 대기하지 않음**

### ⚡ OSSemQuery()

현재 세마포어의 상태를 알 수 있음

<aside>

**Q. 그냥 전역변수를 사용해도 되는데, 왜 굳이 semaphore를 사용해야 하나요?**

**mutual exclusion을 보장하기 위함**입니다. microC OS는 kernel mode가 따로 없기 때문에 global variable을 사용해도 무방하지만, 이 경우 해당 함수의 `reenterancy`가 보정되지 않습니다.

</aside>

## 세마포어 생성

```c
OS_EVNET *OSSemCreate(INT16U cnt)
{
    OS_EVENT *pevent;
    OS_ENTER_CRITICAL();
    pevent = OSEventFreeList;
    if (OSEventFreeList != (OS_EVENT *)0)
    {
        OSEventFreeList = (OS_EVENT *)OSEventFreeList->OSEventPtr;
    }
    OS_EXIT_CRITICAL();
    if (pevent != (OS_EVENT *)0)
    {
        pevent->OSEventType = OS_EVENT_TYPE_SEM;
        pevent->OSEventCnt = cnt;
        OSEventWaitListInit(pevent);
    }
    return (pevent);
}
```

```c
    OS_ENTER_CRITICAL();
    pevent = OSEventFreeList;
    if (OSEventFreeList != (OS_EVENT *)0)
    {
        OSEventFreeList = (OS_EVENT *)OSEventFreeList->OSEventPtr;
    }
    OS_EXIT_CRITICAL();
```

자유 ECB list로부터 ECB를 얻는다. OSEventFreeList의 ECB 포인터는 다음 ECB를 가리키도록 조정.

```c
    if (pevent != (OS_EVENT *)0)
    {
        pevent->OSEventType = OS_EVENT_TYPE_SEM;
        pevent->OSEventCnt = cnt;
        OSEventWaitListInit(pevent);
    }
```

이 때 pevent는 이미 자신만의 ECB가 할당된 상태.

### OSSemCreate()가 return되기 직전의 ECB

![image.png](image%2036.png)

## 세마포어 대기

```c
void OSSemPend(OS_EVENT *pevent, INT16U timeout, INT8U *err)
{
    1. pevent가 가리키는 ECB가 세마포어인지 검사한다.(pevent->OSEventType)
    2. 세마포어가 사용가능한 경우(cnt > 0) count를 1 감소시키고 리턴
    3. 세마포어가 사용가능하지 않은 경우(cnt = 0)
        다른 태스크가 세마포어에 시그널을 보낼 때까지 sleep
        3.1 sleep 하도록 하기 위해 태스크의 TCB의 상태를 OS_STAT_SEM로 만듦
        3.2 timeout값의 지정(OSTCBCur -> OSTCBDly = timeout)
        3.3 태스크를 sleep 상태로 만든다.(OS_EventTaskWait())
    4. OS_Sched() 호출하여 우선 순위 높은 태스크를 실행시킨다.
    5. 다시 이 태스크가 실행될 때 timeout이 발생한 것이면,
        OSEventTO() 함수가 호출되고, ECB에의 링크가 끊어진다.
}
```

task가 wait()하고 있는 상태. 여기서 `timeout`이 되거나 `signal`을 받았다면 다시 ready 상태가 된다.

### timeout

- task가 timeout되었다면, `OSEventTO()`함수가 호출되고, **ECB에의 링크가 끊어진다.**

### Semaphore

반면 semaphore에 의해 pend 상태가 종료되었다면 어떤 task가 semaphore를 post하였다는 의미입니다.

## 세마포어 대기

```c
void OSSemPost (OS_EVENT *pevent)
{
    1. pevent가 가리키는 ECB가 세마포어 인지 검사한다. (pevent->OSEventType)
    2. 세마포어를 기다리는 태스크가 있다면
        2.1 그중 우선순위가 가장 높은 태스크를 ECB 대기리스트에서 제거하고는 실행 상태를 만든다. <OS_EventTaskRdy() 함수 호출>
    2.2 OS_Sched()을 호출한다.
    3. 세마포어를 기다리는 태스크가 없다면 세마포어 counter를 1 증가시킨다.
}
```

```c
INT16U OSSemAccept (OS_EVENT *pevent)
{
    1. 세마포어 count가 0보다 클 때만 1감소되고 return한다.
    2. OSSemAccept()를 호출한 부분에서는 return값을 확인하여, 0 이 아닌 경우에만 자원을 할당받아 사용한다.
}
```

<aside>

**Q. OSSemAccept는 OSSemPost와 어떤 점에서 다른가요?**

둘 다 semaphore를 post하지만, `OSSemAccept`는 `OSSemPost`와 달리 wait을 하지 않습니다.

</aside>