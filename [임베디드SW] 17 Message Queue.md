# [임베디드SW] 17. Message Queue

<aside>

# 💖 Message Queue

</aside>

하나 이상의 변수를 전달할 수 있음. 주로 memory address를 전달함.

## Queue control block

![image.png](image%2033.png)

- `OSEventPtr` section에 `OS_Q`의 주소를 넣어 가리키게 함.
- `OS_Q`는 다시 msgTbl을 가리킴
    - 이렇게 하면 msg table의 크기가 가변적이더라도 `OS_Q` 라는 event control block의 크기는 고정시킬 수 있음.

## Free list

![image.png](image%2034.png)

message queue ECB도 역시 미리 free list로 만들어 놓고 관리함.

## Circular buffer

![image.png](image%2035.png)

- **FIFO**
- message data는 memory 상의 circular buffer 안에 저장된다.
- **queue control block과 실제 message의 주소는 다르다.** 그 주소만 이 circular buffer에 들어가는 듯…??

## Operations

### Message queue 생성

```c
#define MSG_QUEUE_SIZE 20
void *MsgQueueTbl[20];
MsgQueue = OSQCreate(&MsgQueueTbl[0], MSG_QUEUE_SIZE);
```

### OSQPend

Message queue에 메시지 오기를 기다림.

- 만약 queue 속에 저장되어 있던 message가 따로 없다면?
    - 새로운 메시지가 도착하거나, timeout 시간이 지날 때까지 태스크를 sleep 시키고는 scheduler(OSSched())를 호출한다.
    - 이는 mailbox와 동일한 방식이다.

### OSQFlush

message queue에 저장되어 있는 모든 mesage 삭제.

- 이를 통해 queue를 초기화시킬 수 있다.

### OSQPostOpt

mailbox에서와 마찬가지로, message를 모든 waiting list의 tast들에 broadcast할 수 있는 option을 넣을 수 있다.