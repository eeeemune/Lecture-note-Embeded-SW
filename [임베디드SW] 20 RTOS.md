# [임베디드SW] 20. RTOS

<aside>

# 💖 실시간 스케줄링

</aside>

## 임베디드에서의 스케줄링

임베디드에서는 ROTS를 구현해야 하기 때문에 FIFO와 같은 방법을 사용하지 않는다고 한다.

## 실시간 스케줄링의 종류

- RMS
- EDF

## RMS, EDF의 전제

아래의 모든 전제들이 지켜져야 RMS, EDF가 제대로 작동한다.

1. 모든 task는 periodic하다.
2. 모든 task는 independent하다.
    1. task1이 끝나야 task2 실행 가능… 이딴 거 없다고 가정
3. Each deadline ends with its period.
4. 실행 시간은 static하게 정해져 있다.
5. 모든 task가 preemtive하다.
6. scheduling overhead는 없다.