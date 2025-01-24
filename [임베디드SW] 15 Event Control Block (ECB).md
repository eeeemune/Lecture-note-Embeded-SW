# [임베디드SW] 15. Event Control Block (ECB)

<aside>

# 💖 OS: Event control block

</aside>

## Event control block이란 무엇인가요?

<aside>

**ECB(Event Control Block)** 

일종의 우체통처럼, task들 사이의 통신을 담당하는 기능을 수행함.

</aside>

## Event Control Block의 역할

![image.png](image%2027.png)

1. task들 사이의 통신을 manage
2. **timer 기능**

## ECB의 구조

![image.png](image%2028.png)

### OSEventType

ECB의 용도를 나타내는 field. 이게 semaphore인지, mailbox인지, mutex인지…

### OSEVENTCnt

세마포어 카운트를 저장.

### OSEVENTPtr

- 메일박스나 메시지 큐로 전달할 때, 해당 구조체를 가리키는 포인터.
    - **메일박스나 메시지 큐일 때만 사용**한다.

### OSEventGrp, OSEventTbl[]

![image.png](image%2029.png)

- 이벤트 발생을 기다리는 task들이 여러개일 수 있음
- 이 때 그 task들을 list-up해서 관리

### bits

task들의 priority를 table로 관리하고 있음.

## ECB가 우선순위를 scheduling하는 방법

### Insert Task to event wait list

```c
pevent -> OSEventGrp != OSMapTbl[prio>>3];
pevent -> OSEventTbl != OSMapTbl[prio & 0x07];
```

### Remove Task From event wait list

**나중에 채워넣기 !! !!**

## Free Event Control Block List

![이런 식으로, ECB들은 singly linked list로 관리된다.](image%2030.png)

이런 식으로, ECB들은 singly linked list로 관리된다.

- ECB를 그 때 그 때 만들어서 할당하면 시간이 오래 걸림
- 하지만 real-time OS를 만들기 위해서는 시간이 중요함
- 그래서 `OS_INIT` 이 호출되었을 때 ECB를 다 만들어 두고 미리 `free event control block list`에 연결해 둠
- 그리고 semaphore나 mailbox queue가 생성되면 free event control block에서 삭제되고 초기화 시킴
    - 할당되는 **ECB의 개수는 application에서 어떤 걸 요구하느냐에 따라 다름**

## ECB Operations

- ECB 관련하여 사용할 수 있는 operations들…

![image.png](image%2031.png)

우리는 위에 있는 함수들을 가져다 사용하지만, 그 함수들은 내부적으로 아래의 함수들을 이용해 구현되었다고 하심

### ⚡ OS_EventWaitListInit(OS_EVENT *pevent)

ECB 초기화 (ECB마다 waiting list가 있음)

- 새로 만들었을 때 호출
- **이게 호출되었다는 건 ECB를 기다리는 task가 한 개도 없다는 의미**

### ⚡ OS_EventTaskRdy()

태스크를 준비 상태로 만들기(signal, post 등)

- signal을 받았을 때 호출됨
- ECB를 기다리고 있던 task들 중 **가장 우선순위가 높은 task**를 `ready` 상태로 바꿈

### ⚡ OS_EventWait()

태스크를 어떤 이벤트에 대해 wait 상태로 만들기.

- task를 ready list에서 삭제하고 wait list에 새로 insert

### ⚡ OS_EventTO()

이벤트를 기다리다가 **timeout이 발생하여**, 태스크를 `ready` 상태로 만들기

## MicroC implements

### OS_EventTaskRdy()

```c
void OS_EventTaskRdy(OS_EVENT *pevent, void *msg, INT8U msk)
{
    y = OSUnMapTbl[pevent->OSEventGrp];
    bity = OSMapTbl[y];
    x = OSUnMapTbl[pevent->OSEventTbl[y];
    bitx = OSMapTbl[x];
    prio = (INT8U) ((y>>3) + x);
    if ((pevent->OSEventTbl[y] &= ~bitx) == 0){
        pevent->OSEventGrp &= ~bity;
    }
    ptcb = OSTCBPrioTbl[prio];
    ptcb->OSTCBDly = 0;
    ptcb->OSTCBEventPtr = (OS_EVENT *) 0;
    //ptcb->OSTmsg = msg; (해당 ECB가 Q혹은 mbox 일 경우)
    ptcb->OSTCBStat &= ~msk;
    if (ptcb->OSTCBStat == OS_STAT_RDY) {
        OSRdyGrp |= bity;
        OSRdyTbl[y] |= bitx;
    }
}
```

1. 가장 우선순위가 높은 task 찾기

```c
    y = OSUnMapTbl[pevent->OSEventGrp];
    bity = OSMapTbl[y];
    x = OSUnMapTbl[pevent->OSEventTbl[y];
    bitx = OSMapTbl[x];
    prio = (INT8U) ((y>>3) + x);
```

- 가장 우선순위가 높은 task 찾기.
- 이 task의 x, y좌표가 각각 저장되는 듯,,,??
1. wait list에서 삭제하기

```c
    if ((pevent->OSEventTbl[y] &= ~bitx) == 0){
        pevent->OSEventGrp &= ~bity;
    }
```

- ECB 대기 리스트에서 삭제하기
1. 해당 priority를 가진 tcb를 찾기

```c
    ptcb = OSTCBPrioTbl[prio];
    ptcb->OSTCBDly = 0;
    ptcb->OSTCBEventPtr = (OS_EVENT *) 0;
```

1. ready list에 insert하기

```c
    ptcb->OSTCBStat &= ~msk;
    if (ptcb->OSTCBStat == OS_STAT_RDY) {
        OSRdyGrp |= bity;
        OSRdyTbl[y] |= bitx;
    }
```

### OS_EventTaskWait()

```c
void OS_EventTaskWait(OS_EVENT *pevent){
    OSTCBCur->OSTCBEventPtr = pevent;
    if ((OSRdyTbl[OSTCBCur->OSTCBY] &= ~OSTCBCur->OSTCBBitX) == 0){
        OSRdyGrp &= ~OSTCBCur->OSTCBBitY;
    }
    pevent->OSEventTbl[OSTCBCur->OSTCBY] |= OSTCBCur->OSTCBBitX;
    pevent->OSEventGrp |= OSTCBCur->OSTCBBitY;
}

```

**실행 중인 task를 wait 상태로 돌리기**

- 우선, 현재 어떤 task가 실행 중이라는 것은 그 task가 가장 priority가 가장 높다는 의미.
1. ECB를 현재 TCB와 연결하기
    1. `pevent` : Event control block의 address
2. ready list에서 자신을 삭제
3. Event의 waiting list에 자신을 집어넣음

### OS_EventTO

```c
void OS_EventTO(OS_EVENT *pevent)
{
    if ((pevent->OSEventTbl[OSTCBCur->OSTCBY] &= ~OSTCBCur->OSTCBBitX) == 0)
    {
        pevent->OSEventGrp &= ~OSTCBCur->OSTCBBitY;
    }
    OSTCBCur->OSTCBStat = OS_STAT_RDY;
    OSTCBCur->OSTCBEventPtr = (OS_EVENT *)0;
}
```

**event의 timer 설정하기**

- event를 기다리면서 `wait` 상태에 있다가, **timeout이 되면 wait lsit로부터 삭제**됨.
1. ECB를 대기 list에서 삭제
2. 해당 TCB가 event를 가리키지 않도록 `null`로 초기화