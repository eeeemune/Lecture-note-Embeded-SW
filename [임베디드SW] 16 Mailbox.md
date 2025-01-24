# [임베디드SW] 16. Mailbox

<aside>

# 💖 Mailbox

</aside>

## Operations

### mailbox 생성

```c
OS_EVENT *OSMboxCreate (void *msg)
```

- msg: message를 가리키고 있는 pointer
- 내부적으로 `pevent->OSEventPtr = msg;` 같은 code가 있어서, EventPtr이 **보내고자 하는 msg의 address를 가리키도록 함**

### mailbox 삭제

```c
OS_EVENT *OSMboxDel (OS_EVENT *pevent, INT8U opt, INT8U *err) 
```

<aside>

**Mailbox를 삭제하려면 그 Mailbox를 액세스할 수 있는 모든 Task를 먼저 삭제해야 함**

</aside>

### mailbox에서 메시지 기다리기

```c
void *OSMboxPend (OS_EVENT *pevent, INT16U timeout, INT8U *err)
{
void *msg;
if (OSIntNesting > 0) { /* See if called from ISR ... */
*err = OS_ERR_PEND_ISR; /* ... can't PEND from an ISR */
return ((void *)0);
}
#if OS_ARG_CHK_EN > 0
if (pevent == (OS_EVENT *)0) { /* Validate 'pevent' */
*err = OS_ERR_PEVENT_NULL;
return ((void *)0);
}
if (pevent->OSEventType != OS_EVENT_TYPE_MBOX) { /* Validate event block type*/
*err = OS_ERR_EVENT_TYPE;
return ((void *)0);
}
#endif
OS_ENTER_CRITICAL();
msg = pevent->OSEventPtr;
if (msg != (void *)0) { /* See if there is already a message */
pevent->OSEventPtr = (void *)0; /* Clear the mailbox */
```

- mailbox에 message가 존재하지 않을 경우, message가 도착할 때까지 기다려야 함
    - 이 때 `OSMboxPend`라는 함수를 호출하게 됨.
        - 이 경우 해당 task는 wait 상태가 됨.
        - 그러다가 그 task가 다시 HPT(Highest Priority Task)가 되었을 때, 이 task가 다시 **wait 상태로 돌아감**
    - `EventPtr`의 값이 **NULL이 아닌 값을 가리키고 있을 때**, message가 있다고 판단함.
- `timeout` option을 통해서 TO를 설정할 수 있음
    - 만약 `timeout == 0`일 경우 wait forever
- mailbox에서 message를 읽었다면, 그 message를 보낸 task한테 신호를 보내줘야 함.
    - 그러면 신호를 받은 task는 다시 ready 상태가 됨.
    - 만약 깨어났는데 message가 null이라면?
        - timeout이 일어났다는 것. 뭔가 잘못됐다는 의미…

### mailbox에 message 보내기

```c
INT8U OSMboxPost (OS_EVENT *pevent, void *msg)
{
#if OS_ARG_CHK_EN > 0
if (pevent == (OS_EVENT *)0) { /* Validate 'pevent' */
return (OS_ERR_PEVENT_NULL);
}
if (msg == (void *)0) { /* Make sure we are not posting a NULL pointer */
return (OS_ERR_POST_NULL_PTR);
}
if (pevent->OSEventType != OS_EVENT_TYPE_MBOX) { /* Validate event block type */
return (OS_ERR_EVENT_TYPE);
}
#endif
OS_ENTER_CRITICAL();
if (pevent->OSEventGrp != 0x00) { /* See if any task pending on mailbox */
OS_EventTaskRdy(pevent, msg, OS_STAT_MBOX);
/* Ready highest priority task waiting on event */
OS_EXIT_CRITICAL();
OS_Sched(); /* Find highest priority task ready to run */
return (OS_NO_ERR);
}if (pevent->OSEventPtr != (void *)0) {
/* Make sure mailbox doesn't already have a msg */
OS_EXIT_CRITICAL();
return (OS_MBOX_FULL);
}
pevent->OSEventPtr = msg; /* Place message in mailbox */
OS_EXIT_CRITICAL();
return (OS_NO_ERR);
} 
```

- OSMboxPost 함수는 **이미 기다리고 있는 task가 있다면 그걸 깨워주는 역할까지 수행**한다.
- `if (pevent->OSEventGrp != 0x00)`
    - `OSEventGrp` 이 0이 아니라면, mailbox와 연관되어 wait되고 있는 어떤 task가 존재한다는 의미이다.
- `if (pevent->OSEventPtr != 0x00)`
    - OSEventGrp이 0이 아니라는 것은, **이미 mailbox가 무엇인가로 차있다는 의미**이다. ****

### 기다리지 않고 mailbox에서 메시지 읽기

```c
void *OSMboxAccept (OS_EVENT *pevent)
{
void * msg;
OS_ENTER_CRITICAL();
if (pevent->OSEventType != OS_EVENT_TYPE_MBOX) {
OS_EXIT_CRITICAL();
return ((void *)0);
}
msg = pevent->OSEventPtr;
if (msg != (void *)0) {
pevent->OSEventPtr = (void *)0;
}
OS_EXIT_CRITICAL();
return (msg);
}
```

현재 `OSEventPtr`에 있는 message를 전달함. 만약 message가 nullptr이어도 그냥 그대로 return함.

### mailbox에 대한 정보만 얻기

```c
INT8U OSMboxQuery (OS_EVENT *pevent, OS_MBOX_DATA *pdata)
```

## Mailbox 동작 예시

![image.png](image%2032.png)

### Msg가 있을 경우

1. 우선 task1과 task2가 통신하기 위해서 ECB를 하나 만듦.
    
    ```c
    TxMbox = OSMboxCreate((void *)0);
    ```
    
2. task1에서 `OSMboxPost` 호출: message 보냄
3. 나중에 task2가 실행되다가 `pending`이라는 함수를 호출한다면,
    
    ```c
    char *rxmsg;
    INT8U err;
    rxmsg = (char *)OSMboxPend(TxMbox, 0, &err); 
    ```
    
4. 기다리고 있을 필요가 없기 때문에 바로 실행됨

### Msg가 없을 경우

1. 우선 task1과 task2가 통신하기 위해서 ECB를 하나 만듦.
2. task1에서 `OSMboxPost` 호출: message 보냄
3. 나중에 task2가 실행되다가 `pending`이라는 함수를 호출한다면,
4. 일단 Msg가 없기 때문에 **waiting 상태로 전환됨**

## OSMboxPostOpt()

Option을 추가할 수 있는 mailbox post api.

- Mailbox에 **대기 중인 모든 Task에게 메시지를 Broadcast**할 수 있는 기능을 추가 가능하다.

```c
if (pevent->OSEventGrp != 0x00) { /* See if any task pending on mailbox */
if ((opt & OS_POST_OPT_BROADCAST) != 0x00) {
/* Do we need to post msg to ALL waiting tasks ? */
while (pevent ->OSEventGrp != 0x00) {
/* Yes, Post to ALL tasks waiting on mailbox */
OS_EventTaskRdy(pevent, msg, OS_STAT_MBOX);
}
} else {
OS_EventTaskRdy(pevent , msg, OS_STAT_MBOX);
/* No, Post to HPT waiting on mbox */
}
OS_EXIT_CRITICAL();
OS_Sched(); /* Find highest priority task ready to run */
return (OS_NO_ERR);
}
```

- 위의 그림처럼, waiting list에 있는 모든 ECB들에 전부 message를 전달.
    - 그러면 waiting list에 있던 모든 ECB들이 다 ready 상태로 전환됨

## Mailbox의 한계

- 하나의 pointer만 보낼 수 있음. 그래서 한 개 이상의 변수를 전달하고 싶다면 **message queue를 이용함.**