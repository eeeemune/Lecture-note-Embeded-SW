# [임베디드SW] 3. Embeded 운영체제

<aside>

# 💖 임베디드 운영체제

</aside>

## 임베디드 SW의 역사

### 초기의 임베디드 시스템

주로 간단한 firmware가 주류여서 전자공학과 애들이 걍 짬…

### 시기 별 코드 길이의 변화

시간이 갈 수록 엄청나게 복잡해짐. 따라서 임베디드 SW만의 OS가 필요해짐.

![image.png](image%207.png)

### 임베디드 시스템 전용 OS의 등장

임베디드 시스템의 특성 상 **실시간성을 보장**해야 할 일이 많기 때문에 일반 운영체제로는 한계가 있음. 그래서 **실시간 OS**라는 특별한 OS가 등장함.

## 널리 사용되는 임베디드 OS

### 1. RTOS

- **VxWorks**
    - Honda의 Asimo
- **Nucelsus PLUS**
    - 휴대폰 단말기, PDA, 우리별 1호/2호에 사용됨.
- **REX**
    - 퀄컴에서 만듦
- **자동차용 embedded OS**
    - ccOS(현대, GV60), mbOS(벤츠)
    - Android automotive (르노, 혼다, 포드)

### 2. 임베디드 리눅스

<aside>

**임베디드 리눅스**

임베디드 시스템용 Linux

</aside>

- 저성능 프로세서, 소용량 메모리를 가짐
- 장점
    - 단위 모듈로 설계
    - 완전한 운영체제
    - 오픈소스임
    - 기존 desktop linux와 개발 환경이 동일함 ⇒ 개발이 용이함.
- 단점
    - **그 자체로는 Real-time을 지원하지 못함**
    - 기존의 RTOS보다 많은 메모리를 요구함
    - 오픈소스이기 때문에 제품화하려면 솔루션 구성이 어려움.

<aside>

**Linux가 실시간성을 지원하지 못하는 이유**

- **==`Interrupt latency`때문.**
- Linux는 기본적으로 `interrupt`에 의해 여러 프로세스를 실행함.
- CPU 속도는 엄청나게 빠르지만, **interrupt는 키보드 입력 등 외부 입력에 의해 실행되므로 느릴 수밖에 없음**.
- 따라서 Linux는 latency가 있을 수밖에 없음.
- 교수님이 중요하다고 말씀하심… 칠판까지 써 가면서 설명하심.
</aside>

### 3. 기타

- iOS, Andriod 등…
- 최근 각광받는 OS들이라고 한다.

<aside>

# 💖 μC/OS-II

</aside>

<aside>

**Micro-Controller Operating Systems**

실시간 운영체제의 일종. 오픈 소스이고 상업적 용도로 사용도 가능한데 항공기에도 사용될 정도로 안정적이라고 한다.

</aside>

- 우리 실습 때 배울 거라고 말씀하신다…

## micro C의 특징

![image.png](image%208.png)

User mode와 Kernel mode가 분리되지 않았다… kernel에서 칼춤 춘다