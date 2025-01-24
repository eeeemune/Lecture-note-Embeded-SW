# [임베디드SW] 21. Rate Monotonic Scheduling (RMS)

<aside>

# 💖 Rate Monotonic Scheduling(RMS)

</aside>

## RMS의 도입 배경

- Priority가 **static하게 정해진다고 가정**했을 때,
- 주기가 더 짧은 task에게 더 높은 우선순위를 주자 !!
    - deadline이 더 빨리 찾아오기 때문

### 예시

- Period가 50인 `T1`과 100인 `T2`가 있다고 하자.
    
    ![image.png](image%2045.png)
    
1. ⭕ `T1`에 높은 priority를 할당한다고 했을 때,
    
    ![image.png](image%2046.png)
    
2. ❌ `T2`에 높은 priority를 할당한다고 했을 때,
    
    ![image.png](image%2047.png)
    
    이런 식으로 **dealine을 어기는 상황**이 발생하게 된다.
    

## RMS이란?

채워넣기

## Feasiblity

어떤 task가 `feasible`하다는 것은 무엇일까?

<aside>

**Feasiblity** 

어떤 task set이 Feasible하다면, **반드시 deadline 내에 모두 실행될 수 있음**

</aside>

## CPU Utilization

![image.png](image%2048.png)

위의 식을 만족한다면 feasible한 task set이다.

### RMS의 CPU Utilization과 feasiblity

![image.png](image%2049.png)

위처럼, 0.69정도에 수렴 → **0.7정도면 feasible**하구나 !! !! 라는 사실이 일반적으로 알려져 있음

### 주의) Utilization 식은 필요충분조건이 아니라 필요조건이다

- Utilization 식을 만족하면 **반드시 feasible**하다.
- 하지만, 만족하지 않더라도 그 식이 feasible할 수도 있다.
    - 그래서 utilization 식을 만족하지 않을 경우 그 task set이 feasible할지 아닐지는 직접 계산해봐야 알 수 있다…

## RMS 방식의 문제점

### 예시 상황

- task 1, 2가 있다. 우선순위는 1이 더 높다.
- 이 때 task 2의 deadline까지 3 남아있다. task 2가 실행되고 있다.
- 이러한 상황에서 CP 5짜리 task 2가 preemtive한다면?
- **task2는 deadline을 넘기게 된다!**
- 위와 같은 상황을 방지하기 위해 `EDF 방식`이 생김