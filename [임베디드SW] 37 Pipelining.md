# [임베디드SW] 37. Pipelining

<aside>

# 💖 Pipelining

</aside>

## Background: Patterson’s Laundry

어떤 코인 세탁소가 있다고 합시다. 이 코인 세탁소에는 세탁기, 건조기, 빨래 접는 곳이 각각 한 개씩 있다. 이 때 세탁기 사용 시간은 30분, dry 시간은 40분, fold 시간은 20분이라고 합니다. 이러한 상황에서, 코인 세탁소에 방문한 고객이 여러명이라면 아래와 같이 효율적으로 사용할 수 있을 것입니다.

![image.png](image%2072.png)

### pipelining의 필요성

만약 사용자가 무한히 많은 상황이라고 가정한다면,

- pipelining을 사용하지 않았을 경우, 인 당 110분이 걸리게 됩니다.
- pipelining을 사용했을 경우, 평균적으로 인 당 40분 내에 처리가 가능합니다.

## Pipeling이 사용되는 instruction들

### ⚡ Fetch

이 instruction을 memory에서 register로 가져오세요

### ⚡ Decode

이 operation이 어떤 기능을 해야 하는지 decoding해주세요

## Instruction time의 길이와 pipelining

### Cortex-A7

![image.png](image%2073.png)

pipeline이 별로 없음…

### Cortex-A15

![image.png](image%2074.png)

### instruction을 쪼개서 pipelining이 길어질 수록 좋은가요?

<aside>

**Q. instruction을 쪼개서 pipelining이 길어질 수록 성능이 좋아지나요?**

그렇지 않습니다. 모든 isntruction이 독립적이지는 않기 때문입니다.

</aside>

- 어떤 operation에서 활용되는 변수가 그 이전 operation을 끝낸 후에 사용되어야 하는 경우에는 해당 operation을 기다릴 수밖에 없습니다.
- stage가 많아질 수록 위의 이유로 기다리는 시간이 오히려 더 많이 필요하게 됩니다.
- 그래서 요즘은 30stages정도에서 더이상 stage를 쪼개지 않는 추세입니다.

<aside>

# 💖 ARM ISA

</aside>