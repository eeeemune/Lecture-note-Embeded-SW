# [임베디드SW] 22. Earliest Deadline First (EDF)

<aside>

# 💖 Earliest Deadline First- EDF

</aside>

## EDF 방식

- 무조건 deadline이 더 가까운 task를 가장 먼저

## EDF 방식의 장단점

### 장점

- CPU를 전부 다 쓸 수 있다.
- 아래의 식만 만족하면 **무조건 feasible하다.**
    
    ![image.png](image%2050.png)
    
    - CPU가 utilization이 100%만 안 된다면 deadline을 절대 넘기지 않을 수 있다.

### 단점

- task 하나가 끝날 때마다 무조건 deadline을 계산한다 → overhead 발생