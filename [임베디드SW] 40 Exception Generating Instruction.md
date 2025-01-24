# [임베디드SW] 40. Exception Generating Instruction

<aside>

# 💖 Exception-generating instruction

</aside>

강제로 interrupt를 발생시킵니다. 보통 system call을 호출할 때 쓰이게 됩니다.

## Format

<aside>

 **SWI <immediate_24>**

</aside>

- ‘n번 **s**oft**w**are **i**nterruption을 발생시켜라’ 라는 의미입니다.

## Processor modes

Processor는 여러가지 mode state를 가질 수 있습니다. 이렇게 **process의 상태에 따라 register를 다르게 할당함으로서 불필요한 메모리 접근을 줄일 수 있습니다.**

### ⚡ User mode

보통의 state입니다. 여러개의 mode들 중 유일하게 unprevileged한 mode입니다.

### ⚡ FIQ mode

fast interrupt

### ⚡ IRQ mode

General purpose interrupt handling

### ⚡ Supervisor mode

OS를 위한 protected 모드를 제공합니다. linux의 kernel mode같은 것이라고 생각하면 이해가 쉽습니다.

### ⚡ Abort mode

순차적으로 할 때는 PC를 자동으로 증가시키겠지만, memory를 따라갔더니 잘못된 주소가 나왔다면 abort mode로 진입하게 됩니다.

### ⚡ Undefined mode

## Registers

위에서 우리는 load하고 store할 때 register0에서 register15까지 사용 가능하다고 배웠습니다. 이는 **한가지 모드에서 접근 가능한 register의 개수가 16개**이기 때문입니다. 실제로는 37개의 register가 존재합니다.

### ⚡ General purpose Registers

**31개**. 보통의 register. 32bit이고, PC를 포함하고 있다.

![image.png](image%2085.png)

- **banked register**
    - 위의 그림에서 파란색으로 표시된 부분입니다.
    - banked register는 **해당 mode만 사용할 수 있습니다**.
        - fiq같은 경우에는 빠르게 처리해야 하기 때문에 그 때만 special하게 사용할 수 있는 register를 다른 mode들에 비해 두고 있습니다.
- **Stack Pointer**
    - `R13`
- **Link Register**
    - `R14`
- **Program Counter**
    - `R15`
    - 14, 15번과 다르게 비용상의 문제로 모든 mode들이 공유해서 사용합니다.

### ⚡ Status Registers

**6개.** 

- 32bit이지만 그 중 12개만 접근 가능합니다.
- 1개: Current Program Status Register
- 5개: Saved Program Status Register
    - mode 별로 하나씩 할당됩니다.