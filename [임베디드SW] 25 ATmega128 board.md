# [임베디드SW] 25. ATmega128 board

<aside>

# 💖 ATmega128 보드

</aside>

## 보드 구성도

![image.png](image%2056.png)

## ATmega 128 Board 설명

- ATmega128의 CPU의 성능은 최대 16Mhz입니다.
    - 1초에 16000000번의 clock을 발생시킬 수 있다는 의미입니다.
    
    <aside>
    
    **Mhz** 
    
    1초 당 clock이 **몇 백만 번** 발생하는가?
    
    </aside>
    
    - 16MIPS
- **RISC** 구조의 ISA를 가지고 있습니다.
    
    <aside>
    
    **RISC** 
    
    Reduced Instruction Set Computer. 필수적인 기능만 남긴 instruction set
    
    </aside>
    
    - 무려 133종… 의 ‘간단한’ 명령어 set을 가지고 있다고 합니다.
- 총 53개의 GPIO를 내장하고 있습니다.
    
    <aside>
    
    **GPIO**
    
    General-purpose input/output. 사용자가 직접 입력 포트 또는 출력 포트로 사용할 수 있는 pin.
    
    </aside>
    
- ATmega128 보드는 8개의 외부 인터럽트를 포함한 34개의 인터럽트 벡터를 내장하고 있습니다.

## 메모리

**하나의 메모리** 안에 section이 나눠져 data와 instruction이 담겨 있다.

1. 프로그램 메모리
2. 데이터 메모리

## Resistor Setting

### Pull Up Resistor

![image.png](image%2057.png)

- button input이 0일 때 전류가 통하는 방식의 resistor입니다.
    - 이렇게 setting했을 때 **logic level이 inverted** 됩니다.
- Pin과 **+극 사이에 저항이 위치해 있습니다.**

### Pull Down Resistor

![image.png](image%2058.png)

- 값이 1일 때 전류가 통하는 setting입니다.
- **접지 쪽에 저항이 연결**되어 있습니다.

<aside>

# 💖 ATmega128 임베디드 프로그래밍

</aside>

## 시간 지연 시스템 제공 함수

`_delay_ms(unsigned int i)` , `_delay_us(unsigned int i)`

- AVR 헤더파일에서 제공함

```cpp
#define F_CPU 16000000UL
#include <util/delay.h>
```

- 반복문에 의한 시간 지연은 시스템 시간에 따라서 편차가 크지만, 시스템 제공 함수를 사용하면 그렇지 않음