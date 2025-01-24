# [임베디드SW] 39. Load and Store Instructions

<aside>

# 💖 Load and store instructions

</aside>

## ⚡ Single register data transfer

## ⚡ Block register data transfer

- 2개 이상의 Regester에서 memory로 값을 전달할 수 있습니다.
    - R0~R15까지 접근 가능합니다.
- **이 때 작은 번호의 register는 무조건 더 작은 숫자의 memory로 들어가게 됩니다.**

### 사용 예시

assembly code에서 subroutine을 call하는 상황이라고 가정합시다.

- 이 때 우리는 subroutine을 call하기 전에 **현재 register의 상황을 저장**해 두어야 합니다.
- 그리고 돌아올 subroutine에서부터 돌아왔을 때는 그 마지막 register의 상황을 불러 와야 합니다.
- 이럴 때 우리는 block register data transfer를 수행하게 됩니다.

<aside>

# 💖 Address modes

</aside>

context switching을 할 때 쓰인다고 함

## Address modes

### ⚡ LDMIA

<aside>

**LDMIA R0, {R5-R8}**

</aside>

![image.png](image%2082.png)

**I**ncremental **A**fter. register **`R0`이 가리키고 있는 주소로부터 커지는 방향으로** 4개의 memory가 담고 있는 값을 `R5`, `R6`, `R7`, `R8`에 넣어주세요.

### ⚡ LDMDA

<aside>

**LDMDA R0, {R5-R8}**

</aside>

![image.png](image%2083.png)

Decremental After. register **`R0`이 가리키고 있는 주소보다 작아지는 방향으로** 4개의 memory가 담고 있는 값을 Register 5~8에 넣어 주세요.

### ⚡ LDMIB

<aside>

**LDMIB R0, {R5-R8}**

</aside>

IA와 비슷하지만, **base address의 값을 포함하지 않는다**는 점에서 차이가 있습니다.

- 예를 들어 `R0`이 0x1000을 가리키고 있다면,
- LDMIA instruction을 사용했을 때는 **`R5`의 값이 0x1004의 값**이 됩니다.
- 반면 LDMIB instruction을 사용했을 때는 **`R5`의 값이 0x1000의 값**이 됩니다.

## Stack operations

우리가 위에서 배운 `LDM`, `STR` 등의 intsturction 대신 아래의 명령어를 사용할 수도 있습니다.

- ARM은 Full Descending stack을 default로 사용함

### ⚡ Full stack

stack pointer가 현재 **차 있는 데이터의 top부분**을 가리킵니다.

### ⚡ Empty Stack

stack pointer가 **현재 차 있는 데이터의 바로 다음 부분**을 가리킵니다.

### ⚡ Descending stack

stack이 memory **주소가 감소하는 방향으로 커집니다**.

### ⚡ Ascending stack

stack이 memory **주소가 증가하는 방향으로 커집니다.**

### 예시: Full Descending stack

Full stack의 특성과 Descending stack의 특성을 가집니다.

- ARM complier의 **default 설정은 FD method**입니다.

![image.png](image%2084.png)

## STMFD, LDMFD for subroutine

assembly 차원에서 함수를 어떤 식으로 호출하는지 알아 봅시다. 우리가 이러한 코드를 짜지 않아도 컴파일러가 아래의 코드를 자동으로 삽입합니다.

### Call a subroutine

<aside>

**STMFD R13!, {R0-R12, LR}**

</aside>

- subroutine을 call하기 전에 현재에 있는 모든 register들의 상태를 memory에 저장해야 합니다.
- 따라서 `R0`~ `R12`와 `LR`를 FD method로 storing합니다.
    - `R13`은 Stack Pointer입니다.
        
        <aside>
        
        **Link Register**
        
        subroutine이 끝난 이후 돌아갈 주소를 담고 있는 register
        
        </aside>
        
    - 현재 Stack Pointer가 가리키는 **memory 주소 다음부터 14 word에 register들의 상태를 저장**합니다.

### Return from a subroutine

<aside>

**LDMFD R13!,{R0-R12, PC}** 

</aside>

- subroutine으로부터 **원래 context에 돌아가려면 register들의 상태 또한 원래대로** 되돌려야 합니다.
    - 우리는 subroutine 호출 전 마지막 register의 상태를 미리 memory에 저장해 두었습니다.
- 위의 코드는 다음과 같이 register를 설정하는 것과 같습니다.
    - `R13` ← PC
    - `R12` ← 그 직전 memory의 값