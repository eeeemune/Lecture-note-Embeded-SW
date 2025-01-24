# [임베디드SW] 2. Real-time Embeded System

<aside>

# 💖 실시간 임베디드 시스템

</aside>

## Real-time system의 정의

<aside>

**Real-time System**

**정해진 시간 내에** 결과를 출력하는 시스템 

</aside>

- 대부분의 임베디드 시스템은 real-time system 요소를 내포한다. 아닌 경우가 거의 없을 정도…
- 그래서 용어 상 실시간 시스템과 임베디드 시스템을 혼용해서 사용하기도 한다.

## Real-time system의 특징

### Timeliness

<aside>

**Timeliness**

적시성. 요청한 그 시간 내에 처리가 되는 것. **더 빨라도** 더 느려도 안 된다.

</aside>

결과 산출에 걸리는 시간에 적시성을 가짐.

### Predictablity

<aside>

**Predictablity**

예측 가능성. 모든 외부 자극에 예측 가능한 방식으로 대응할 수 있는 것.

</aside>

- `predictable`
    - 나올 수 있는 **모든 입력들에 대해 대응책**이 있어야 한다.
    - 컴퓨터처럼 예상치 못한 입력이 있을 때 그냥 무시한다… 이런 식으로는 안 된다.

## 실시간 시스템의 분류

| **hard real-time system** | soft real-time system |
| --- | --- |
| 데드라인을 어기면 절대 안 됨 | dead line 맞추면 좋은데, 못 맞추면 어쩔 수 없음… |
| 원자력 발전소, 항공기, 우주 왕복선 등 | DVD 재생기, 네트워크 관련 기기 등 |

### Hard real-time system 의 단점

1. OS가 불완전함
    1. 실시간성을 반드시 보장하기 위해서 시간이 많이 들어가는 작업들을 전부 없앰. 따라서 불완전할 수밖에 없음.
2. 대부분 thread model로 실행됨
    1. 속도를 위해서 **프로세스가 아니라 thread 모델로 실행**됨.
    2. 따라서 memory protection이 없음
        1. 자신만의 memory가 없다 == `memory protection`이 없다.
        2. 모두가 memory들을 공유하기 때문에 한 개의 task에서만 오류가 나도 남의 공간을 전부 침범하기 때문에 **전체 시스템이 다운됨**
3. 특정 회사 혹은 기능에 따라 개발
    1. 구매해서 사용하려면 비쌈…