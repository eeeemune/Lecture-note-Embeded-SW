# [임베디드SW] 36. ARM Architecture

# 💖 ARM Architecture

## ISA

<aside>

**ISA** 

Instructions Set Architecture. 어떤 processor가 처리할 수 있는 명령어의 집합.

</aside>

## ARM

가장 널리 쓰이는 processor를 만드는 회사. 휴대폰, Raspberry pi 등 다양한 기기에 ARM사의 processor가 사용됩니다.

### ARM Architecture의 특징

- 주로 32bit 기반
- RISC architecture
    
    <aside>
    
    **RISC** 
    
    Reduced Instruction Set Compiler. CISC(Complex…)의 반대 개념
    
    </aside>
    

### RISC Architecture

ARM은 RISC architecture를 채택하여 사용합니다.

| 장점 | 단점 |
| --- | --- |
| 하드웨어를 간단하게 설계할 수 있음 | 명령어를 조합해서 컴파일해야 하기 때문에 어셈블리어가 굉장히 복잡해짐 |
- 요즘은 CISC의 시대는 저물고 RISC로 넘어가는 추세라고 합니다.

## System-on-chip

하나의 칩에 Processor 뿐만이 아니라 여러 부품이 들어있는 형태를 가리킵니다. Processor를 포함하여 RAM, GPU등이 하나의 chip을 공유합니다.

## ARM architecture의 특징

### ⚡ 간단한 하드웨어

register의 형태를 전부 똑같이 만들어서 하드웨어의 형태를 간단하게 구현하였습니다.

- ARM architecture의 가장 큰 특징입니다.

### ⚡ Load/store architecture

ARM Architecture에서는 **오직 load/store instruction만 memory에 access**합니다.

- 나머지 모든 operation은 register에 넣어 둔 data들을 통해서만 수행됩니다.

### ⚡ Fixed-length instruction fields

모든 instruction의 binary 상 길이는 동일합니다.

## Endianess

![image.png](image%2068.png)

instruction을 memory에 잘라 저장할 때, chunk 단위에서 오름차순으로 저장하는지 내림차순으로 저장하는지에 관한 방식 차이입니다.

- ARM architecture는 big endian 방식과 little endian 방식을 모두 지원합니다.
    - 이러한 특징을 middle-endian이라고 합니다.

### ⚡ Little-endian memory system

![image.png](image%2069.png)

instruction의 **LSB부터** memory에 잘라 저장합니다.

- 대표적으로 intel사에서 활용하는 방식입니다.
- little-endian 방식의 장점
    - 16bit 데이터를 가져올 때 8bit씩 나눠서 가져올 수 있습니다.

### ⚡ Big-endian memory system

![image.png](image%2070.png)

instruction의 **MSB부터** memory에 잘라 저장합니다.

### The effect of endianness

![image.png](image%2071.png)

- LDRB r2, [r1]
    - R1으로부터 1byte만 읽어서 R2에 넣겠다