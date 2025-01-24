# [임베디드SW] 34. Timer와 Prescaler

<aside>

# 💖 Timer

</aside>

## 8비트 타이머

8bit로 이루어져 있는 timer입니다. 0부터 255까지 셀 수 있습니다.

- ATmega128의 timer 중 0번, 2번 타이머는 8bit timer입니다.
- 8bits기 때문에 0~255까지 셀 수 있습니다.
    - 그 이상은 overflow가 발생하게 됩니다.

### ATmega128에서의 카운터(타이머)

1. 타이머 0, 2 : 8비트 타이머
2. 타이머 1, 3 : 16비트 타이머

## Prescaler

system에서 발생한 clock의 주기를 더 느리게 조정할 수 있는 장치입니다.

### Prescaler의 활용: 예시

- 예를 들어, 만약 prescaler의 값이 1024로 설정되어 있다면 clock의 주기가 1024배 길어집니다.
    - 마치 1024번의 clock이 발생했을 때 1번의 clock이 발생한 것처럼 동작한다는 의미입니다.
- 10bit prescaler라면, `2^10 = 1024` 배까지 설정할 수 있습니다.

### Prescaler의 등장 배경

**Prescaler는 system clock의 frequency가 지나치게 빠르기 때문에 등장**하였습니다. system clock이 1번 발생했을 때는 너무 짧은 시간이 흐르기 때문에 **system clock이 n번 발생했을 때 마치 한 번 clock이 발생한 것처럼 동작**하도록 설정하는 것입니다. **이 때 n값을 prescaler**라고 합니다.

## 8비트 타이머관련 레지스터

### ⚡ TCCRn

`TCCRn` register를 활용하면 어떤 prescaler를 활용할지 결정할 수 있습니다.

- `TCCRn` register의 0, 1, 2번째 bit를 활용하여 prescaler의 종류를 선택합니다.

| CS02(index 2) | CS01(index 1) | CS00(index 0) | 설명 |
| --- | --- | --- | --- |
| 0 | 0 | 0 | 클록 입력 차단 |
| 0 | 0 | 1 | Prescaler 사용 x |
| 0 | 1 | 0 | 8분주 |
| 0 | 1 | 1 | 32분주 |
| 1 | 0 | 0 | 64분주 |
| 1 | 0 | 1 | 128분주 |
| 1 | 1 | 0 | 256분주 |
| 1 | 1 | 1 | 1024분주 |

### ⚡ TCCTn

Timer/Counter Register n. 직접 count를 세는 register입니다.

- 분주된 주기가 지날 때마다 값이 1씩 증가합니다.
- interrupt가 걸리면 값이 0으로 clear됩니다.

### ⚡ TIMSK

Timer Interrupt Mask. 타이머에 대한 interrupt를 Enable시킬 수 있습니다. 

| 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| OCIE2  | TOIE2   | TICIE1 | OCIE1A | OCIE1B  | TOIE1  | OCIE0  | TOIE0 |
- `TOIEn` : Time over interrupt enable
    - 1로 설정되었다면 **n번 timer가 꽉 찼을 때 interrupt**를 발생시킵니다.
- `OCIEn` : 출력값 비교
    - 1로 설정되었다면 n번 timer값의 출력을 비교할 수 있습니다.

### ⚡ TCNTn

카운터 값 세팅

## 예제: 타이머로 원하는 시간을 세팅하기

직접 timer를 이용하여 원하는 시간을 세팅해 봅시다. 우리의 미션은 다음과 같습니다.

<aside>

**미션**  l  100us 후에 interrupt를 발생시키기

</aside>

### 1. 1clock에 몇 초 걸리는지?

- 16MHz clock이고 prescaler 값이 32라고 합시다.
    - system clock은 16000000Hz입니다.
    - prescaler값이 32이므로 주기가 32배 길어집니다.
        - 같은 1 진동수를 위해 시간이 32배 더 많이 필요하다는 의미입니다.
- 이 때 1clock에 필요한 시간은 2us입니다.
    - (system clock) 1/16000000 = 0.0625
    - (prescaler) 0.0625*32 = 2

### 2. 100us만큼의 시간이 되려면 몇 clock이 필요한지?

- 100us만큼의 시간이 되려면 **50clock이 필요**합니다.
    - 1clock에 2us만큼 소요되기 때문입니다.

### 3. TCNT에 원하는 값 저장

- `TCNT` register에 `256 - 50 = 206`값을 미리 할당해 둡니다.
- 그러면 clock이 50번 더 발생한 후에 overflow가 발생할 것입니다.
- `TCNT` register에서 overflow가 발생했을 때 우리가 원하는 interrupt가 발생합니다.

<aside>

# 💖 예제 프로그램

</aside>

스위치를 한 번씩 누를 때마다 한 음계씩 높아지는 프로그램을 설계해 봅시다.

## 1. ‘도’ 음 내기

```c
#include <avr/io.h> 
#include <avr/interrupt.h> 
#define ON 1 
#define OFF 0 
#define DO_data 17 
volatile int state = OFF; 
 
ISR(TIMER0_OVF_vect) { 
 if (state == ON) { 
  PORTB = 0x00; 
  state = OFF; 
 }
  else {
  PORTB = 0x10; 
  state = ON;
 } 
 TCNT0 = DO_data; 
} 
int main() { 
 DDRB = 0x10; 
 TCCR0 = 0x03; // 32분주 
 TIMSK = 0x01; // Overflow 
 TCNT0 = DO_data; 
 sei(); // 전역 인터럽트 
 while(1); 
} 
```

### clock 주기 구하기

- ‘도’음의 주파수는 1046.6Hz임
    - 이는 1초에 1046.6번 진동한다는 의미
- 주파수를 이용해 주기를 구하기
    - 1/1046.6 = 956us
- 478us동안 ON, 477us동안 off

### 카운터 세팅

- Prescaler로 32분주를 선택하여 주기를 2us로 만듬
- 478us/2us = 239 클록을 카운팅해야 하므로 `256-239=17`값을 TCNT0에 Write
- TIMSK의 TOIE0(Overflow 인터럽트) 설정 및 전역인터럽트 설정

## 2. 레, 미, 파… 를 위한 TCNT0값 구하기

| 음계 | 도 | 레 | 미 | 파 | 솔 | 라 | 시 | 도 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Hz | 1046.6 | 1174.6  | 1318.6  | 1397.0  | 1568.0  | 1760.0  | 1795.6 | 2093.2 |
| `TCNT0` | 17 | 43 | 66 | 77 | 97 | 114 | 117 | 137 |