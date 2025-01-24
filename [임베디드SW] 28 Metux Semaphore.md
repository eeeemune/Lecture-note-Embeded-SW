# [임베디드SW] 28. Metux Semaphore

<aside>

# 💖 Mutex Semaphore

</aside>

uCOS-II에서는 Mutex 기능을 제공합니다. 위의 그림처럼, **mutex의 기능을 하는 event block**이 두 task 사이의 communication을 가능하게 합니다.

## MicroC OSII에서의 우선순위 계승 프로토콜

- 우선순위 계승 프로토콜을 실제로 구현할 때, 두 task는 **같은 우선순위**를 가져야 합니다.
- 하지만 uCOS에서는 **모든 task의 우선순위가 달라야만 합니다.**
    - 한 개의 task 당 한 개의 우선순위가 부여되기 때문입니다.
- 이 때, uCOS-II에서 우선순위 계승 프로토콜을 구현하가 위해서는 현재 **사용 가능한 우선순위 중 더 높은 우선순위를 할당**합니다.

### 예시

다음과 같은 task가 있다고 가정합시다.

<aside>

Task1: priority 10
Task2: priority 12
Task3: priority 15

</aside>

위와 같은 상황에서, task3이 task1의 priority를 계승해야 한다면 **더 높은 priority인 9를 할당**한다

- 위와 같은 상황에서, Task3이 semaphore 자원을 가지고 있고 **Task1이 해당 자원을 기다리기 위해 bloked** 되었다고 생각해 봅시다.
- 우선순위 계승 프로토콜을 구현하기 위해서는 Task3의 priority를 10으로 올려야 합니다.
    - Task1의 priority가 10이기 때문입니다.
- 하지만 uCOS-II에서 서로 다른 task가 동일한 priority를 가질 수는 없습니다.
- 그래서 **Task3에 priority 9를 할당**합니다.

<aside>

# 💖 uCOS에서의 Mutex Semaphore 구현

</aside>

## Mutex Creation

### 사용

```cpp
OS_EVENT mutex = OSMutexCreate(9, &err);
```

### 구현

```c
OS_EVENT *OSMutexCreate(INT8U prio, INT8U *err)
{
    1. ISR에서의 호출인지 체크, PIP가 OS에서 제공하는 범위에 있는지 체크
    2. PIP의 태스크가 OS에서 사용되고 있지 않은지 확인하고,
       OSTCBPrioTbl[prio] 에 NULL 값이 아닌 값을 넣어서 예약 // PIP == prio
    3. EventFreeList에서 ECB 받음
    4. ECB->OSEventType = OS_EVENT_TYPE_MUTEX;
		   ECB->OSEventCnt = (prio << 8) | OS_MUTEX_AVAILABLE;
	     ECB->OSEventPtr = (void *)0;
	     OSEventWaitListInit(pevent);
    5. Return ECB;
}
```

1. argument가 정당한지 체크합니다.
2. argument로 넘겨받은 priority를 미리 예약합니다.
3. return할 Event Control Block을 세팅합니다.
4. 해당 ECB를 return합니다.

## Mutex Pend

```c
void OSMutexPend(OS_EVENT * pevent, INT16U timeout, INT8U * err) {
  1. validity check
		 ISR에서 호출한 것인가 ?,
     해당 ECB를 OSMutexCreate() 가 생성한 것인가 ? 등
  2. //pevent -> OSEventCnt 하위 8 비트가 0xFF 라면 뮤텍스 사용 가능
  3. if (뮤텍스의 사용이 가능하다면?) 
	   {
		   pevent -> OSEventCnt &= 0xFF00;
	     pevent -> OSEventCnt |= OSTCBCur -> OSTCBPrio;
	     pevent -> OSEventPtr = (void * ) OSTCBCur;
	     return;
		} //event block을 넘겨줌
	//아닐 경우,
  4. 우선 순위 계승 프로토콜 실행
  if (호출한 태스크 우선순위가 뮤텍스 소유자 우선순위보다 높은지 ? ) //우선순위 계승
  {
    뮤텍스 소유자 태스크 우선 순위 = PIP;
    새로운 우선 순위 할당 후 상태 결정 위해
    if (뮤텍스 소유자 태스크가 준비상태 인가 ? ) {
      준비 상태 해제; // 뮤텍스 소유자의 기존 우선순위를 readyQ에서 삭제
      rdy = TRUE;
    } else rdy = FALSE;
    새로운 PIP를 적용하여 TCB 내용 변경;
    if (rdy == TRUE)
      PIP 의 태스크를 준비상태로 설정; // 뮤텍스 소유자의 우선순위를
    높여서 readyQ에 추가
  }
  호출 태스크 상태 -> 뮤텍스 대기 상태
  OSTCBCur -> OSTCBStat |= OS_STAT_MUTEX;
  OSTCBCur -> OSTCBDly = timeout;
  OS_EVENTTaskWait(pevent); // 실제 대기 상태로 만듦
  OS_Sched(); // 준비상태가 아니므로 다음 우선순위의 태스크 호출
  // 대기 시간 만료로 인한 준비 상태로 상태 변화
  if (태스크가 뮤텍스를 기다리는가 ? ) //timeout 에 의해 빠져나온경우
    OS_EVNETO(pevent); // 뮤텍스 대기 리스트에서 삭제
}
```

‘mutex 주세요’ 하는 함수입니다. 함수 내부는 아래와 같이 구현되어 있습니다. 

1. validity check
2. 현재 mutex를 사용가능한지 아닌지 검사
    1. pevent→OSEventCnt의 하위 8비트가 `0xFF`라면 사용 가능한 상태입니다. 
3. 만약 mutex 사용이 가능하다면 ECB setting한 후 return합니다.
    1. eventcnt의 하위 2bytes를 0으로 만들고,
    2. eventcnt의 상위 bit를 priority로 채웁니다
    3. eventptr에 `TCBCur`값을 넣어줍니다
    4. 그리고 return
4. 만약 mutex 사용이 불가능하다면 일단 우선순위 계승 프로토콜을 실행합니다. 
    1. 만약 mutex를 가지고 있는 task보다 나의 task가 높으면 아래의 내용을 수행합니다. 
    2. 현재 mutex를 가지고 있는 task의 우선순위를 나의 priority로 바꿉니다
    3. 으아아 !! !! !! !!
    4. 시험에 나온다면 우선순위 계승 프로토콜 부분이 나올 것 같은데 으악 자 ㄹ모르게스음닝ㄹㄴㄹㄴㅇㄹ

### Mutex Deletion

```c
OS_EVENT *OSMutexDel(OS_EVENT *pevent, INT8U opt, INT8U *err)
{
1. ISR에서의 호출인지 체크, 삭제할 ECB가 존재하거나 뮤텍스가 맞는지 여부 확인
2. 뮤텍스를 기다리는 태스크가 있는지 검사한다.(tasks_waiting 값 갱신)
3. case OS_DEL_NO_PEND: // 아무 태스크도 기다리지 않는 경우 삭제
    if (tasks_waiting == TRUE)
        *err = OS_ERR_TASK_WAITING;
    else
        ECB 반환;
4. case OS_DEL_ALWAYS: // 항상 삭제
    모든 대기중인 태스크를 ready 상태로 변경;
    ECB 반환;
    if (tasks_waiting == TRUE)
        OS_Sched();
    return;
}

```

```
void OSMutexPend(OS_EVENT *pevent, INT16U timeout, INT8U *err)
{
    1. validity check
        ISR에서 호출한 것인가
        ?,
        해당 ECB를 OSMutexCreate() 가 생성한 것인가 ? 등
    2. pevent -> OSEventCnt 하위 8비트가 0xFF라면 뮤텍스 사용 가능 if (뮤텍스의 사용이 가능한가 ?)
    {
        pevent->OSEventCnt &= 0xFF00;
        pevent->OSEventCnt |= OSTCBCur->OSTCBPrio;
        pevent->OSEventPtr = (void *)OSTCBCur;
        return;
    }
아닐 경우 3. 우선 순위 계승 프로토콜
if (호출한 태스크 우선순위가 뮤텍스 소유자 우선순위보다 높은지?) //우선순위 계승
{
    뮤텍스 소유자 태스크 우선 순위 = PIP;
새로운 우선 순위 할당 후 상태 결정 위해
 if (뮤텍스 소유자 태스크가 준비상태 인가?)
{
    준비 상태 해제; // 뮤텍스 소유자의 기존 우선순위를 readyQ에서 삭제
    rdy = TRUE;
}
else rdy = FALSE;
새로운 PIP를 적용하여 TCB 내용 변경;
if (rdy == TRUE)
    PIP 의 태스크를 준비상태로 설정; // 뮤텍스 소유자의 우선순위를
높여서 readyQ에 추가
}
호출 태스크 상태->뮤텍스 대기 상태
    OSTCBCur->OSTCBStat |= OS_STAT_MUTEX;
OSTCBCur->OSTCBDly = timeout;
OS_EVENTTaskWait(pevent);           // 실제 대기 상태로 만듦
OS_Sched();                         // 준비상태가 아니므로 다음 우선순위의 태스크 호출
                                    // 대기 시간 만료로 인한 준비 상태로 상태 변화
 if (태스크가 뮤텍스를 기다리는가?) //timeout 에 의해 빠져나온경우
     OS_EVNETO(pevent);             // 뮤텍스 대기 리스트에서 삭제
}
```