# [임베디드SW] 10. Ready List

<aside>

# 💖 Ready list

</aside>

- `ready list`는 **ready 상태에 있는 task들만 bitmap 상태로 관리**한다.
    
    <aside>
    
    **Q. 왜 ready list는 bitmap 형태로 구현되어 있나요?**
    
    bitmap을 사용한다면 linked list에 비해 공간은 cost가 높지만 시간 cost는 압도적으로 낮습니다. ready list는 **O(n) time 안에** **priority를 판별하기 위해** bitmap 형태를 사용합니다.
    
    </aside>
    

## Data structure

### ⚡ OSRdyGrp

![image.png](image%2016.png)

- 8개의 task들이 모여 하나의 그룹을 만든다.
- 그 group을 가리키는  pointer.
- 만약 어떤 group 안에 하나라도 **ready 중인 task**가 있다면, 그 task가 속해있는 group의 bit가 1이 된다.

### ⚡ OSRdyTbl[]

![image.png](image%2017.png)

- 8개의 group이 있고, 각 group 내에는 8개씩 task들이 있다.
- 그 task들을 담고 있는 array.

### ⚡ OSMapTbl[]

![image.png](image%2018.png)

- 위 그림처럼 **미리 mask하기 위한 bit들이 준비되어있는 table.**
    - 이를 이용해서 그 때 그 때 bitshift하지 않아도 되도록 만든다.

### ⚡ OSUnMapTbl[]

## Priority position

![image.png](image%2019.png)

- `Y`: group의 index.
    - **Priority / 8**
- `X`: ready list에서의 x좌표.
    - **Priority % 8**

### 예시 1. OSRdyTbl로 OSRdyGrp 채우기

![image.png](image%2020.png)

### 예시 2. 17번 task가 새로 ready 상태가 되었다면?

- OSRdyTble의 (1, 2)를 1로 만들어야 한다.
    - `osrRdyTbl[2] |= 00000010`
    - 이를 다음과 같이 표현할 수도 있다.
        - `OSMapTbl[1]`

<aside>
💡

**Skill: x/8은 x<<3과 같다.**

그래서 `prio/8` 대신 `prio>>3`으로 써도 놀라지 말자…

</aside>

### 예시 3. 17번 task가 wait 상태로 변했다면?

```cpp
if ((OSRdyTbl[prio >> 3] &= ~OSMapTbl[prio & 0x07]) == 0)
OSRdyGrp &= ~OSMapTbl[prio >> 3];
```

1. **`OSRdyTbl[]`에 0이 한 개도 없다면**, `OSRdyGrp`의 bit도 0으로 바꿔줘야 한다.
2. `OSRdyTbl[]`의 원하는 (x, y)를 0으로 바꾼다.
    1. row: OSRdyTbl[prio >> 3] 
    2. column: ~OSMapTbl[prio & 0x07]

## Find highest proirity task

1. 한  group 내에서 우선순위가 가장 높은 task를 하나씩 찾으려면 시간이 오래 걸림.
2. 이 때 **ready group과 ready table이 가질 수 있는 값의 범위**를 이용함.
    1. [00000000, 11111111] → [0, 255]
    2. 이를 아래의 table로 미리 만들어 놓음…!!
        
        ```cpp
        INT8U const OSUnMapTbl[] = {
        0, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        6, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        7, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        6, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,
        4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0
        };
        ```
        
    3. 예를 들어, `OSRdyGrp`이 01101000이라면,
        1. `OSUnMapTbl[104] == 3`
            1. 3번째 group이 가장 우선순위가 높구나…!
        2. `OSUnMapTbl[3] == 2` 
            1. 3번째 group의 2번째 task가 가장 우선순위가 높구나…!
        3. 우선순위가 가장 높은 task는 26번 task
            1. 3번째 group의 2번째 task
            2. == 3*8+2
            3. == (3<<3) + 2
            4. == 26
    4. 이를 코드로 나타내면 다음과 같음
        
        ```cpp
        y = OSUnMapTbl[OSRdyGrp];
        x = OSUnMapTbl[OSRdyTbl[y]];
        prio = (y << 3) + x; 
        ```