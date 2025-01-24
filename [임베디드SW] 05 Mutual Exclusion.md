# [임베디드SW] 5. Mutual Exclusion

<aside>

# 💖 OS: Mutual exclusion

</aside>

<aside>

**Critical section** 

코드에서 공유자원을 사용하는 부분

</aside>

## 어떻게 Mutual exclusion을 구현할 수 있나요?

### 1. 인터럽트 발생을 방지

```c
void Function (void){
OS_ENTER_CRITICAL(); // 인터럽트 비활성화
// 공유 데이터 엑세스
OS_EXIT_CRITICAL(); // 인터럽트 활성화
}
```

- **아예 인터럽트를 발생시키지 않는 방법**
    - **CPU가 한 개이기 때문에 가능**
        - 따라서 multi-core 환경에서는 불가능한 방식이다
        - microC-II OS는 단일 CPU를 사용한다.
    - 한 개의 task만 계속 실행하게 되기 때문에 공유자원에 여러 개의 task가 실행될 여지가 없다
- `critical section`에 들어가기 전에 `interrupt`를 disable시키고, 나온 후에 다시 enable시킴

### 2. Semaphore

```c
void Function (void){
WAIT();
// 공유 데이터 엑세스
SIGNAL();
}
```

- `semaphore` 를 쥐고 있는 task만 공유자원에 접근 가능하도록 구현
    - `critical section`에 들어가기 전에 `semaphore`를 -1
    - `critical section`에서 나올 때 +1
- `semaphore`==0이면 waiting
    - 이를 SLEEP 상태라고도 한다
        
        <aside>
        
        **Q. 우선순위가 낮은 task가 semaphore를 가지고 있을 때 우선순위가 더 높은 inturrupt가 발생하면 어떻게 되나요?**
        
        A. **우선순위가 더 높은 task라도 기다립니다.** 우선순위가 더 높은 task가 waiting 상태이기 때문에 선점하지 않고 기다리게 됩니다.
        
        </aside>
        
- Binary semaphore VS Counting Semaphore
    - Binary semaphore : 공유자원의 변수가 0,1인 경우
    - Counting semaphore : 1이상의 값

<aside>

**Q. semaphore를 기다리고 있는 task는 어떤 state에 있나요?**

`waiting` 상태에 있습니다. 만약 busy waiting이라면 `running` 상태이지만, microC OS에서 기본적으로 제공하는 WAIT() function을 사용해서 `wait` 상태로 만들 수 있습니다.

</aside>

## Semaphore

### 세마포어 은닉

<aside>

**세마포어 은닉** 

**세마포어를 추상화시켜서 제공**하는 것. sem++이런 식으로 직접 접근하는 것이 아니라 signal()같이 함수 형태로 제공한다.

</aside>

- 예시
    
    ```c
    int CommSendCmd(char *cmd, char *response, int timeout){
        Acquire port's semaphore;
        Send command to device;
        Wait for response(with timeout);
        if (timed out) {
            Release semaphore;
            return (error code);
        } else {
            Release semaphore;
            return (no error);
        }
    }
    ```
    

### Deadlock(교착상태)

<aside>

**Deadlock** 

서로 상대방이 가지고 있는 자원을 요청하고 있어서 둘 다 실행되지 못하고 있는 상태

</aside>