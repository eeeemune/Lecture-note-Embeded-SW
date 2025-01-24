# [임베디드SW] 19. Event Flag

<aside>

# 💖 Event Flag

</aside>

<aside>

**Event Flag**

각 bit들이 특정 이벤트에 대응됨. **이벤트가 발생하면 `ISR`이 특정 bit를 0 또는 1로 설정**함. 여러 개의 task들 사이에서 특정한 event의 발생 여부를 알려주기 위해서 사용.

</aside>

## Event flag들의 조합

![image.png](image%2037.png)

위 그림처럼 `event flag group`이 각각의 event에 대응되는 bit를 가지고 그 bit들을 조합하여 사용한다.

## event flag를 이용한 태스크 동기화 예시

### 1. 우선 순위 높은 태스크를 우선 순위 낮은 2개의 태스크가 특정 시점까지 블록킹하는 경우

![image.png](image%2038.png)

우선순위가 차례대로 낮은 H, M, L, task가 있다. 이 때 H의 우선순위가 가장 높지만 M, L 모두 실행된 이후에 H를 실행하고 싶은 상황을 생각해 보자.

- 이 때 semaphore를 2개 사용할 수도 있지만, 그건 번거롭다.
- 이런 상황에 event flag를 사용해서 `flag_grp`이 `0000011` 이 될 때까지 pend한다면?
    - 이런 식으로 사용할 수 있다!

### 2. 우선 순위 낮은 2개의 태스크를 우선 순위 높은 태스크가 특정 시점까지 블록킹하는 경우

![image.png](image%2039.png)

## Event flag group structure

![image.png](image%2040.png)

### Event flag group

```c
typedef struct
{                         /* Event Flag Group */
    INT8U OSFlagType;     /* Should be set to OS_EVENT_TYPE_FLAG */
    void *OSFlagWaitList; /* Pointer to first NODE of task waiting on event flag */
    OS_FLAGS OSFlagFlags; /* 8, 16 or 32 bit flags */
} OS_FLAG_GRP;

```

### Event Flags Group Node Structure

```c
typedef struct
{                             /* Event Flag Wait List Node */
    void *OSFlagNodeNext;     /* Pointer to next NODE in wait list */
    void *OSFlagNodePrev;     /* Pointer to previous NODE in wait list */
    void *OSFlagNodeTCB;      /* Pointer to TCB of waiting task */
    void *OSFlagNodeFlagGrp;  /* Pointer to Event Flag Group */
    OS_FLAGS OSFlagNodeFlags; /* Event flag to wait on */
    INT8U OSFlagNodeWaitType; /* Type of wait */
} OS_FLAG_NODE;
```

각 task들을 node로 만들어 doubly linked list로 관리. TCB들이 서로 연결되어 있다. 그리고 `OSFlagNodeFlags`(자신이 깨어나는 event flag들의 조건)을 담고 있다. 

## Operations

### ⚡ OSFlagCreate

- 새로운 event flag block을 생성함

### ⚡ OSFlagDel

### ⚡ OSFlagPend

### ⚡ OSFlagCreate

### ⚡ OSFlagCreate

### ⚡ OSFlagCreate

## Flag group을 이용한 task control의 과정…

### 1. OS_Flag_GRP생성

```cpp
OS_FLAG_GRP *e_grp;
INT8U err;
e_grp = OSFlagCreate(0x00, &err); 
```

![image.png](image%2041.png)

### 2.해당 Flag group을 사용하는 task가 생성됨

![image.png](image%2042.png)

![image.png](image%2043.png)

- 여기서 event flag가 0x11: 10001을 담고있다면?
    - ALL: 1과 4 모두
    - ANY: 1이나 4 중 하나
- 바로 실행되거나 대기열에 들어감
    - case 1: task의 event flag 조건이 flag group과 부합할 때 → 바로 실행
    - case 2: task의 event flag 조건이 flag group과 부합하지 않을 때 → node가 되어서 대기열에 들어감

### 3. task가 여러개 생김 → node들이 doubly linked list로 연결

![image.png](image%2044.png)

### 4. 이 때 OS_FLAG_GRP의 FLAGS들에 변화가 생겼다면?

wait list를 다 따라가면서 조건에 맞는 node들을 풀어준다!