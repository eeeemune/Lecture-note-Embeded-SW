# [임베디드SW] 35. CdS Sensor와 A/D Converter

<aside>

# 💖 광센서

</aside>

## CdS 센서

황화 카디늄을 이용한 센서로, 밝기에 따라 동작합니다.

<aside>

**Cds**

황화 카드늄. 광량이 많아지면 저항값이 작아지고 어두워지면 전기저항값이 커지는 특성을 가진 물질. 

</aside>

- 디지털 신호인 buzzer, switch와 달리 **아날로그 신호**라는 특징이 있습니다.

## A/D 컨버터

<aside>

**광센서 자체가 중요한 것이 아니라 이 컨버터가 중요하다고 하심**

</aside>

**아날로그 신호를 digital 신호로 변경**하는 컨버터입니다.

### Resolution

![image.png](image%2067.png)

분해능. 아날로그 신호가 n만큼 늘었을 때 디지털 출력값이 1 늘어난다면 분해능이 n이라고 합니다.

### conversion time

A/D 변환을 수행하는 데에 필요한 시간

## ATmega128 A/D 컨버터 레지스터

### ⚡ ADMUX

A/D 컨버터에 어떤 multiplexer를 사용할지 선택하는 register

### ⚡ ADCSRA

ADC Control and Status Register A. A/D 컨버터 제어 및 상태 register 중 A.

### ⚡ ADCH, ADCL

AD Converter Data Register High/Low. A/D 컨버터의 데이터를 담고 있는 레지스터