# [임베디드SW] 14. Idle Task in Micro-C OS

<aside>

# 💖 Idle Task

</aside>

## Idle Task

- 가장 우선순위가 낮은 task로, OS가 관리하는 task이다.
- 이 Idle Task가 실행되고 있다는 것은 **CPU가 아무런 task를 실행하고 있지 않은 상태**라는 의미이다.

## Statistics Task

- CPU usage를 계산할 수 있는 task
- priority가 2번째로 낮다
    - 그렇기 때문에 이 함수가 실행된다는 것은 CPU에 남는 자리가 있다는 것  == **CPU usage가 100%가 아니라는 것**

### Statistics Task가 CPU usage를 계산하는 방법

<aside>

**그냥 알고만 있으라고 말씀하신다. 다행이다……**

</aside>

![image.png](image%2025.png)

하지만 엄청 열심히 설명하셨다………

<aside>

$OSCPUUsage_{(\%)} = 100*\lgroup1-\cfrac{OSIdleCtr}{OSIdleStrMax} \rgroup$

</aside>

![image.png](image%2026.png)

- microOS-II에서는 아래와 같이 사용한다.
    
    <aside>
    
    $OSCPUUsage_{(\%)} = \lgroup 100-\cfrac{OSIdleCtr}{\lgroup \cfrac {OSIdleStrMax}{100} \rgroup}  \rgroup$
    
    </aside>
    
    - 이렇게 하면 분모의 overflow를 방지할 수 있다
    - 전체가 floating형이 되는 것도 방지할 수 있다 !!