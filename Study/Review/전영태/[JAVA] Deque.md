# Deque

날짜: 2025/05/31
작성자: 전영태

## **Deque**

> 앞(First)과 뒤(Last) **양쪽에서 삽입과 삭제가 모두 가능한 자료구조**
> 

```
// 알고리즘 TEST에 나왔던 Deque 부분
Deque<String> deque = new ArrayDeque<>();
```

---

## **특징**

| **특징** | **설명** |
| --- | --- |
| **양쪽 삽입 가능** | 앞쪽과 뒤쪽 둘 다에서 데이터를 넣을 수 있음 |
| **양쪽 삭제 가능** | 앞쪽과 뒤쪽 둘 다에서 데이터를 뺄 수 있음 |
| **Queue처럼 사용 가능** | offerLast(), pollFirst() 등 큐처럼 사용 가능 |
| **Stack처럼 사용 가능** | push(), pop()으로 스택처럼 사용 가능 |

---

## **스택처럼 사용**

```
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10); // addFirst와 같음
stack.push(20);
stack.pop();    // 20 제거
```

---

## **큐처럼 사용**

```
Deque<Integer> queue = new ArrayDeque<>();
queue.offerLast(10); // addLast와 같음
queue.offerLast(20);
queue.pollFirst();   // 10 제거
```

---

## **ArrayDeque를 쓰는 이유**

- ArrayDeque는 Deque의 **대표 구현체이다**
- LinkedList보다 성능이 더 좋고, null을 넣을 수 없다
- 스택이나 큐 모두 대신할 수 있어 **범용적이다**