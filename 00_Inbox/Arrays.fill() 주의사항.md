## 1. 문제 상황 (Problem)

`Arrays.fill()`을 사용하여 객체 배열(예: `ArrayList<Integer>[]`)을 초기화하면, 배열의 모든 요소가 **메모리상의 동일한 객체 인스턴스**를 가리키게 된다.

---
## 2. 원인 (Cause)

- Java에서 객체는 **참조(Reference) 타입**이다.
- `Arrays.fill(array, value)`는 전달받은 `value`의 **주소값**을 배열의 모든 칸에 복사한다.
- 결과적으로 모든 배열 인덱스가 하나의 객체를 공유하게 되어, 한 곳에서 데이터를 수정하면 모든 인덱스에서 동일한 수정이 발생한다.

---
## 3. 코드 예시 (Code Example)

### ❌ 잘못된 방법 (오답의 원인)

```
ArrayList<Integer>[] graph = new ArrayList[N + 1];
// 단 하나의 리스트 객체를 생성하여 모든 칸에 주소를 복사함
Arrays.fill(graph, new ArrayList<Integer>()); 

graph[1].add(10);
System.out.println(graph[2].get(0)); // 출력: 10 (graph[2]에도 10이 들어있음!)
```

### ✅ 올바른 방법 (정석)

```
ArrayList<Integer>[] graph = new ArrayList[N + 1];
// 반복문을 통해 각 칸마다 "새로운" 객체를 생성하여 할당함
for (int i = 1; i <= N; i++) {
    graph[i] = new ArrayList<>();
}

graph[1].add(10);
// graph[2]는 비어있으므로 에러 또는 안전하게 관리됨
```

---
## 4. 요약 (Summary)

- **기본 타입 배열**(`int[]`, `boolean[]` 등) 초기화 시에는 `Arrays.fill()`을 써도 무방하다 (값 자체가 복사되기 때문).

- **객체 타입 배열**(`ArrayList[]`, `StringBuilder[]` 등) 초기화 시에는 반드시 **`for` 루프와 `new` 키워드**를 사용하여 독립적인 인스턴스를 생성해야 한다.
