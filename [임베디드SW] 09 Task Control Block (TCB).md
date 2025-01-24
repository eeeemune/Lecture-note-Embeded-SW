# [임베디드SW] 9. Task Control Block (TCB)

<aside>

# 💖 Task Control Block

</aside>

<aside>

**TCB**

task control block. task의 실행에 관련된 모든 정보들이 담겨 있다.

</aside>

- task의 개수만큼 TCB가 존재한다.
- 항상 main memory에 있다.
- `OSTaskCreateExt()`이 호출되었을 때 생성

## TCB의 내부 포인터

### ⚡ OSTCBStkPtr

task가 사용하는 memory stack의 top을 가리킴

### ⚡ OSTCBStkBottom

task가 사용하는 memory stack의 top을 가리킴

### ⚡ OSTCBStkSize

memory공간에서 task가 사용하는 stack의 전체 size를 가리킴

### ⚡ OSTCBDly

timeout이 발생하기 전까지 clock tick이 몇 번 일어났는지에 관한 정보

### ⚡ OSTCBStat

Task state

## TCB들의 관리

- 각 TCB 안에는 `OSTCBNext`, `OSTCBPrev`라는 pointer가 있다.
- TCB들은 doubly linked list로 관리된다.

### Free list

`uCOS_II.h`안에 미리 Free TCB들이 만들어져 있다. 

- compile 시 자동으로 만들어진다.

### OSTBList

- valid한 tcb들이 들어가 있는 곳…

### 예시: 11개의 task를 생성한다면?

1. 11개의 task가 만들어져서 초기화된 후 **freelist에 연결**된다.

![image.png](image%2013.png)

![image.png](image%2014.png)

1. 62, 63번째 task는 **미리 만들어져 OSTCBList에 들어가 있다**
    1. 이는 microC OS에서 기본적으로 만드는 task이다.
    2. priority 또한 가장 낮다. 
2. 아래와 같이, 실행 중인 task들의 priority를 table 안에 저장한다.

![image.png](image%2015.png)

1. 이 때 `OSTCBFreelist`가 실행된다면, Freelist에 있던 task가 OSTCBList로 이동한다.