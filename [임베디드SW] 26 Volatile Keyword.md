# [임베디드SW] 26. Volatile Keyword

## Volatile keyword

`volatile` 컴파일러에게 이 변수를 임의로 최적화하지 말라고 말하는 것

### Sample 1

```cpp
volatile int i, a, b;
a = 0;
b = 0;
for (i = 0; i < 10; i++) {
  b += a * 100;
}
```

volatile이 없다면 a = 0이므로 b+=0으로 최적화 시킵니다. 코드 내에서 a가 0에서 바뀌지 않으므로 그냥 상수 취급해 버립니다.

### Sample 2

```cpp
volatile int a;
a = 10;
a = 20; // 
```

volatile이 없다면 이 코드는 최적화로 인하여 삭제될 수 있습니다. 의미없이 a에 값을 할당하고 사용하지 않기 때문입니다.

### 어떤 상황에 필요한가요?

예를 들어, 다음과 같은 code가 있다고 생각합시다.

```cpp
volatile int i, a, b;
a = 0;
b = 0;
for(i = 0; i<10; i++)
	b += a * 100 //volatile이 없다면 a = 0이므로 b+=0으로 최적화
```

이 경우, a=0으로 고정되어 있으므로 compiler는 `b+=a`가 아니라 `b+=0`으로 최적화합니다.

- 이 때, 사실 다른 task에서 **a의 값을 외부 인터럽트로 받아 오도록 설계**했다면? 이 코드는 제대로 작동하지 않습니다.
- 이러한 상황에 `volatile` 키워드를 사용하게 됩니다.