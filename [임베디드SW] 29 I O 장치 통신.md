# [임베디드SW] 29. I/O 장치 통신

<aside>

# 💖 Precessor와 장치 간 Communication

</aside>

![image.png](image%2061.png)

## BUS

장치를 연결하는 선들의 집합

![image.png](image%2062.png)

- 예를 들어, 어떤 processor의 명령어 길이가 32bit라면 32개의 bus가 필요한 셈입니다.
    - 각각 하나의 bit를 담당해서 0 또는 1을 표시해야 하기 때문입니다.

## Processor와 주변 장치의 통신

### ⚡ Port-based I/O

프로세서가 여러 개의 port를 가지고 있습니다. 주변장치는 그 **port들을 register처럼 읽고 씁니다**. Port-based I/O는 이 port들을 이용하여 I/O를 수행합니다.

### ⚡ Bus-based I/O

특정 **명령어를 통해** 읽기 및 쓰기를 수행합니다.

## Bus-based I/O

<aside>

**시험에 나올 수도 있다고 말씀하심**

</aside>

processor와 주변 장치가 명령어를 통해 커뮤니케이션하는 방식입니다.

- memory-mapped 방식과 I/O mapped 방식으로 나뉩니다.
    - Bus 방식의 I/O는 명령어를 통해서 통신하기 때문에 **주변 장치가 각각 어디에 있는지 logical한 주소가 기록된 공간**이 필요합니다.
    - 이 공간이 memory 안에 있는지, 아니면 따로 공간을 차지하는지에 따라 나뉩니다.

### ⚡ Memory-mapped I/O

I/O 장치의 주소가 **memory의 주소 공간 중 일부를 차지**하는 방식입니다.

- 하나의 memory를 사용하고, 주변 장치들의 주소가 memory 공간의 일부를 차지합니다.

| **장점** | **단점** |
| --- | --- |
| 주변 장치에 접근하도록 할 때 memory 명령어인 LOAD, STORE등의 명령어를 그대로 쓸 수 있음 | 주소 디코딩이 복잡함 |

<aside>

**Q. 왜 memorry-mapped I/O방식에서는 주소 디코딩이 복잡한가요?**

I/O 입출력 장치는 상대적으로 적기 때문에 짧은 주소로도 디코딩할 수 있지만, **상대적으로 크기가 큰 memory와 같은 주소 형식을 사용해야 하기 때문에 불필요하게 복잡한 디코딩**이 필요합니다.

</aside>

### ⚡ I/O mapped I/O(Standard I/O)

I/O 장치의 주소가 적힌 곳을 따로 만들어 두는 방식입니다.

- bus에 부가적인 PIN을 만들어 두어서, 그 **PIN의 state에 따라 현재 가리키고 있는 주소가 memory인지 I/O인지를 판단**합니다.
- **입출력만을 위한 특정한 명령어**가 필요하다는 특징이 있습니다.
    - memory-mapped I/O 방식에서는 `load`, `store`등 memory에 접근하는 주소를 입출력장치에 접근할 때도 동일하게 사용할 수 있지만, I/O mapped I/O방식에서는 `IN`, `OUT`등의 명령어를 추가적으로 사용해야 합니다.