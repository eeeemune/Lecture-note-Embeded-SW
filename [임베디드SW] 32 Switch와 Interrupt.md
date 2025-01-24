# [임베디드SW] 32. Switch와 Interrupt

<aside>

# 💖 스위치를 활용한 interrupt

</aside>

## 임베디드 시스템에서 외부 입력을 확인하는 방법

### 1. while문을 이용해서 계속 확인하기

아래와 같이 프로그램이 실행될 때마다 port input을 확인할 수도 있습니다. 일견 무식해 보이기도 하지만, 실제로 자주 사용되는 방법입니다.

```cpp
#include <avr/io.h>

int main(void) {
  DDRA = 0xff;
  DDRE = 0x00;
  while (1) {
    if ((PINE & 0x10) == 0x00)
      PORTA = 0xff;
    else
      PORTA = 0x00;
  }
}
```

### 방법 2. interrupt 사용

우리가 지금부터 배울 내용입니다.

## interrupt 발생을 알아채는 trigger

### ⚡️ edge trigger

입력 신호가 0→1, 혹은 1→0으로 **변경될 때**를 interrupt trigger로 사용합니다.

### ⚡️ level trigger

입력 신호가 **일정 시간동안 0 또는 1로 유지**될 때를 trigger로 사용합니다.

<aside>

# 💖 ATmega128에서의 Interrupt 활용

</aside>

## Register setting

### ⚡️ EIMSK

External Interrupt MaSK register. INTn번째 port를 활성화 시킬 수 있습니다.

| 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| INT7 | INT6 | INT5 | INT4 | INT3 | INT2 | INT1 | INT0 |

예를 들어,

```c
EIMSK = 00000001;
```

EIMSK register를 위와 같이 설정하였다면, 0번째 bit만 활성화 되어있으므로 INT0 interrupt를 활성화시킵니다.

### ⚡️ EICR

External Interrupt Control Register. INTn번째 PIN에 어떤 일이 일어났을 때 ISR이 trigger될지를 결정할 수 있습니다.

- EICRA와 EICRB로 나뉩니다.
    - EIRCA: PIN0~PIN3까지를 관리
    - EIRCB: PIN4~PIN7까지를 관리
- 예시: `EICRA`
    
    
    | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
    | --- | --- | --- | --- | --- | --- | --- | --- |
    | ISC31 | ISC30 | ISC21 | ISC20 | ISC11 | ISC10 | ISC01 | ISC00 |
    
    | ISCn1 | ISCn0 | 설명 |
    | --- | --- | --- |
    | 0 | 0 | INTn의 Low level에서 인터럽트 발생 |
    | 0 | 1 | 예약(안 씀) |
    | 1 | 0 | INTn의 하강 에지에서 인터럽트 발생 |
    | 1 | 1 | INTn의 상승 에지에서 인터럽트 발생 |

예를 들어, 다음과 같은 코드를 작성하였다고 가정합시다.

```cpp
EIRCA = 00000011
```

- ‘INT0 pin에서 상승엣지가 발생할 때 interrup를 trigger해주세요’ 라는 의미입니다.

### ⚡️ ISR 등록

예를 들어서, `INT4`번 interrupt가 발생했을 때의 ISR이라면

```c
ISR(INT_vect4){
	//service routine 작성
}
```