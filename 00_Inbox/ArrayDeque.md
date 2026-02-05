## 1. 개요

Java에서 선형 자료구조인 스택(Stack)과 큐(Queue)를 사용할 때, 전통적인 `Stack` 클래스나 `LinkedList`보다 **`ArrayDeque`** 사용이 권장된다. 이는 설계상의 결함 해결과 성능 최적화라는 두 가지 측면을 모두 만족하기 때문이다.

---
## 2. 왜 Stack 클래스를 쓰면 안 되는가? (Legacy의 한계)

### ① 설계상의 오류 (상속 관계)

- **문제:** `java.util.Stack`은 `Vector` 클래스를 상속받았다.

- **결과:** LIFO(Last-In-First-Out) 구조여야 함에도 불구하고, 부모 클래스인 `Vector`의 메서드를 사용하여 **중간에 있는 값을 인덱스로 접근하거나 삭제**할 수 있다. 이는 객체지향의 **캡슐화 원칙**을 위반한다.

### ② 불필요한 동기화 (성능 저하)

- `Stack`의 모든 메서드는 `synchronized` 처리가 되어 있어 멀티스레드 환경이 아닐 때도 불필요한 락(Lock)을 건다. 이는 단일 스레드 기반의 알고리즘 문제 풀이나 일반적인 상황에서 상당한 성능 손실을 야기한다.

---
## 3. ArrayDeque의 장점

- **성능:** 내부적으로 가변 배열을 사용하며, `LinkedList`처럼 노드 객체를 매번 생성할 필요가 없어 메모리 오버헤드가 적고 속도가 빠르다.

- **다목적성:** `Deque`(Double-Ended Queue) 인터페이스를 구현하므로 스택과 큐의 역할을 모두 수행할 수 있다.

- **안전성:** 동기화를 지원하지 않으므로 단일 스레드 작업에서 최상의 성능을 낸다. (멀티스레드 환경이 필요하다면 별도의 `Collections.synchronizedDeque` 등을 사용한다.)

---
## 4. 실전 사용 가이드 (Code Snippets)

`ArrayDeque`를 사용할 때 `First`와 `Last`를 명시적으로 사용하면 데이터의 흐름을 더 직관적으로 파악할 수 있습니다.

#### ① 스택(Stack)으로 사용 시 (LIFO)

스택은 한쪽 끝에서만 데이터가 입출력됩니다. `First`를 입구이자 출구로 통일하여 사용합니다.

```
// Deque 인터페이스를 사용하여 양방향 메서드에 접근
Deque<Integer> stack = new ArrayDeque<>();

stack.addFirst(10); // push: 맨 앞에 추가
stack.addFirst(20);

// peekFirst()는 비어있을 때 null을 반환하여 안전합니다.
System.out.println(stack.peekFirst()); // 맨 위 확인 (20)
System.out.println(stack.removeFirst()); // pop: 맨 위 제거 및 반환 (20)
```

#### ② 큐(Queue)로 사용 시 (FIFO)

큐는 뒤로 들어가서 앞으로 나오는 구조입니다. `Last`로 넣고 `First`로 빼는 흐름을 명시합니다.

```
// Deque 인터페이스를 사용하여 양방향 메서드에 접근
Deque<Integer> queue = new ArrayDeque<>();

queue.addLast(10); // offer: 맨 뒤에 추가
queue.addLast(20);

// peekFirst()는 가장 먼저 들어온(가장 오래된) 데이터를 가리킵니다.
System.out.println(queue.peekFirst()); // 맨 앞 확인 (10)
System.out.println(queue.removeFirst()); // poll: 맨 앞 제거 및 반환 (10)
```
