# [임베디드SW] 31. Performance Calculating

<aside>

# 💖 Performance

</aside>

## Background

![image.png](image%2064.png)

위 표에서, CPU speed와 core만 보면 inter coer 7의 경우 cortex보다 6배정도 빠를 것 같지만 **실제로는 20배 이상 속도 차이가 납니다.**

- CPU Time이 다르기 때문에 이런 일이 일어나는 것입니다.
- 이 CPU Time이란 무엇인지, 또 어떻게 계산하는지를 살펴 봅시다.

## CPU execution time for a program

어떤 프로그램을 돌렸을 때 **CPU가 얼마나 일했는가?**

<aside>

`CPU execution time` = `CPU clock cycles` * `Clock cycle time`

- CPU가 돌아간 시간 =
</aside>

- clock cycle time = 1/clock rate

아래의 예제 풀어보기

<aside>

A embedded machine: 100 MHz, program : 10 sec.
B embedded machine: ? , program : 6 sec., but 1.2 times clock cycles
CPU clock cycles for a program/ (100*10^6 cycles/sec.) = 10 sec.
CPU clock cycles for a program = 1000 *10^6 cycles
CPU time B = 1.2*1000 *10^6 cycles / x (clock rate for B) =6 sec.
clock rate for B = 200 MHz

</aside>

## CPI

평균적으로 instruction 하나를 수행하는데 몇 clock cycle이 필요한가?

- 당연히 프로그램마다 CPI는 다르다

<aside>

CPU clock cycle = Instruction for a program * CPI

</aside>

<aside>

**PPT에 있는 예제 풀어보기 !! !!**

</aside>

![image.png](image%2065.png)

## MIPS

<aside>

**시험문제 내기 딱 좋다고 하심 !! !!**

</aside>

<aside>

**MIPS**

1초 당 몇 개의 instruction을 처리하는가?

</aside>

- CPU time을 알면 가장 좋겠지만, 항상 측정할 수는 없는 노릇이다
- 그래서 MIP가 도입되었음

<aside>

CPU time = Instruction count / (MIPS*10^6)

</aside>

<aside>

**PPT에서 위의 공식 유도 과정 직접 해보기 !! !!**

</aside>

- MIPS도 프로그램마다 다를 수밖에 없지만, 대표적인 프로그램 몇 개를 이용해 MIPS를 측정함

### Mflops, Gflops, Petaplops

- floating point instruction에 최적화 되어있음