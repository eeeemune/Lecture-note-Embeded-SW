# [임베디드SW] 6. Inter-Task Communication (ITC)

<aside>

# 💖 OS: 태스크 간 통신

</aside>

## 태스크끼리 어떻게 정보를 주고 받을 수 있나요?

### 1. Global variable

`global variable`을 하나 만들어서 공유 변수로 사용함

- exclusive access해야만 한다 !! !!
    - 위에서 배웠던 intrurrpt disable, semaphore등의 방법을 사용할 수 있다

### 2. message passing

Mailbox, queue, pipe 등 사용

- 셋 중 어떤 **종류를 사용할지는 보통 message의 크기에 따라** 결정한다고 말씀하심