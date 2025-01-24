# [임베디드SW] 30. Interrupt

<aside>

# 💖 인터럽트

</aside>

![image.png](image%2063.png)

- processor와 외부 장치는 **interrupt를 통해서 통신**합니다.
- RTOS에서는 외부에서 들어오는 입력에 대해 실시간으로 반응해야 할 필요성이 있기 때문에 interrupt가 더욱 중요합니다.

### interrupt 발생 시 대응 시나리오

<aside>

1. interrupt가 발생하면 해당 register 값이 바뀝니다.
2. 그러면 바로 ISR이 돌면서 어떤 장치에서 발생시킨 interrupt인지를 판단해야 합니다.
    1. 이 때 이를 판단하기까지 **새로운 interrupt의 발생을 block시킵니다.**
3. 그 다음에 해당 interrupe에 반응합니다.
</aside>

<aside>

# 💖 Atmega 128 에서의 GPIO

</aside>

## GPIO

우리가 사용하는 임베디드 프로세서는 Atmega128입니다.

- Atmega 128같은 경우, **실제로는 chip에 IO port가 장착**되어 있습니다.
    - port-based I/O 방식인 셈입니다.
- 하지만 속도 제어를 위해 별도의 register를 제공하고 memory 주소를 할당해 두었습니다.
    - 따라서 bus-based I/O 방식 중 memory-mapped I/O 방식처럼 동작합니다.
- 그래서 **동작을 위해서는 GPIO의 `memory map` 정의를 활용**합니다.