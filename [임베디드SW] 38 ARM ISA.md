# [임베디드SW] 38. ARM ISA

<aside>

# 💖 ARM ISA

</aside>

## Condition bit

![image.png](image%2075.png)

모든 instruction은 4bit의 condition bit field를 가지고 있습니다.

- 이 bit가 어떤 Opcode인지에 따라서 해당 instruction의 동작이 달라집니다.

### CPSR

Current Processor State Register. ALU의 연산 결과에 따라서 값이 바뀝니다.

- CPU에 CPSR은 단 한 개만 존재합니다.

![image.png](image%2076.png)

## ⚡ Data processing instructions

### Data processing instruction의 종류

- Arithmetic operation
- Logical operation
- Comparison operation
- Movement of data between registers

### Standard format

```nasm
ADD r0, r1, r2       //r0 = r1+r2
```

- **r1 부분은 언제나 register** 입니다.
- r2는 register일 수도 있고 immediate value가 될 수도 있습니다.

### Instruction encoding

![image.png](image%2077.png)

### 예시

<aside>

**ADD r0, r1, r2**

</aside>

![image.png](image%2078.png)

- cond: `1110`
    - 무조건 연산해라
- OP code: `0100`
    - add operation
- conditional field: `0`
    - status register를 바꿔야 하는 경우에는 `1`이 되고, 아닌 경우 `0`으로 남아있음.

<aside>

**ADD r0, r1, r2 LSL #1**

</aside>

![image.png](image%2079.png)

<aside>

**Logical shift left** 

Arithmetic shift라면 MSB가 1, 0… 순환해서 sign bit가 바뀌지만 Logical은 그건 남겨 둔다고 함. 찾아보자…

</aside>

## ⚡ Branch instructions

### Standard format

![image.png](image%2080.png)

### unconditional branch

MIPS의 jmp instruction처럼, `unconditional branch`는 instruction을 이동합니다.

- 함수, return 등이 여기에 해당합니다.

### Instruction encoding

- L: Link bit
    - 1: 현재 주소를 저장해 두고 jump합니다.
    - 0: 현재 주소 저장하지 않고 jump합니다.
- signed immediate_24
    - 현재 PC의 값과 다음 주소 사이의 gap이 적혀 있습니다.
        - 현재 PC(Program Counter)에는 현재 실행 중인 instruction의 memory 주소가 담겨 있습니다.

### 예시

<aside>

**미션** ㅣ assembly로 10번 loop를 실행 후 탈출하는 코드를 짜봅시다.

</aside>

```nasm
MOV r0, #10
SUBS r0, #1
BNE LOOP
```

- `SUBS`: substraction을 진행하고, 결과에 따라 status bit를 바꾸세요.
    - `R0` register의 값이 0이 되면 `BNE LOOP`를 넘어가게 됩니다.

<aside>

**미션** ㅣ An unconditional Jump

</aside>

```nasm
B Label
```

<aside>

**미션** ㅣ Call a subroutine

</aside>

```nasm
BL SUB
...
MOV PC, r14
```

## ⚡ Status register transfer instructions

### MSR과 MRS 명령어

- MRS
    - Move Register to Status register
- MSR
    - Move Status register to Register

### CPSR

![image.png](image%2081.png)

### Example

```nasm
MRS r0, CPSR 
```

- R0 register의 값을 그대로 CPRS에 옮깁니다.

```nasm
MSR CPSR_f, r0 
```

- CPRS의 상위 4개 bit를 r0로 옮깁니다.
- CPRS_F
    - 상위 4개 bit

## ⚡ Load and store instruction

### LDR / STR

하나의 register를 load/store합니다.

### LDM / STM

여러 개의 data 집합을 load, store합니다.

### SWP

memory 와 register의 single data를 swap합니다.

- semaphore에 쓰이기도 합니다.