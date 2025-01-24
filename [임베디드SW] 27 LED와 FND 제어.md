# [임베디드SW] 27. LED와 FND 제어

<aside>

# 💖 LED 제어

</aside>

![image.png](image%2059.png)

## ATmega128 보드의 LED

### LED 작동 원리

- LED 신호에 1을 인하가면 켜진 채로 유지되고, 0을 인가하면 꺼진 채로 유지됩니다.
- 이를 위해서, 다음과 같은 과정으로 LED를 켭니다.

<aside>

1. PA 포트를 output으로 정함. 여기에 output값 1을 할당
    1. `DDRA` 레지스터의 해당 bit에 1을 write.
        1. 방향을 ‘출력’상태로 만드는 것
2. `PORTA` 레지스터의 해당 비트에 1을 write
</aside>

## FND

- Flexible Nemeric Device. 디지털 시계처럼 생긴 LED를 `FND`라고 합니다.
    - 보통 7-Segment LED라고 칭하기도 합니다.

![image.png](image%2060.png)