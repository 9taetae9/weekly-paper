# 🚀 HashSet 내부 동작 방식 및 중복 제거 메커니즘

## 🛠️ 1. HashSet의 주요 특징
- 중복 요소를 허용하지 않음 : 데이터 동등성 확인을 위한 `equals()`, `hashCode()` 메서드 필요
- 순서를 보장하지 않음 : 인덱스가 매개 변수로 넘어가는 메서드(`get(int index)`, `indexOf(Object o)` 등) 없음
- `null` 요소 허용
- 비동기(non-synchronized) 구현
- O(1) 시간 복잡도의 기본 연산 제공

## 🔍 2. 내부 구조

### 🗂️ 2-1) 주요 필드
- `HashSet`은 내부적으로 `HashMap`을 사용하여 구현됨
- `HashSet`은 요소들을 `HashMap`의 key로 저장함
- 모든 값에 대한 value로는 동일한 더미 객체(`PRESENT`)를 사용함 (키의 존재 여부만 확인하면 되기 때문)

### ⚙️ 2-2) 생성자
1. `HashSet()` : 기본 생성자. 데이터를 저장할 수 있는 16개의 공간과 0.75의 로드 팩터를 갖는 객체 생성
2. `HashSet(Collection<? extends E> c)` : 매개 변수로 받은 컬렉션 객체의 데이터를 `HashSet`에 저장
3. `HashSet(int initialCapacity)` : 매개 변수로 받은 개수만큼의 데이터 저장 공간과 0.75의 로드 팩터를 갖는 객체 생성
4. `HashSet(int initialCapacity, float loadFactor)` : 첫 매개 변수로 받은 개수만큼의 데이터 저장 공간과 두 번째 매개 변수로 받은 로드 팩터를 갖는 객체 생성

### 🧠 2-3) 메모리 구조
- `HashMap`은 다음과 같은 구조로 구성됨
    - 버킷 배열 (`Node<K,V>[]`)
    - 각 버킷의 연결 리스트 또는 트리 구조
    - 메타데이터 (크기, 수정 횟수 등)

## ❌ 3. HashSet의 중복 제거 방식

### 🛠️ 3-1) 동작 방식
`HashSet`의 중복 제거 방식을 알아보기 위해 `add()` 메서드 살펴봄

#### HashSet - `add()` 메서드
- `HashSet`은 새로운 요소를 추가할 때 `HashMap`의 `put()` 메서드를 호출함

#### HashMap - `put()` 메서드
- 동일한 키가 이미 존재하면 키의 대응하는 이전 값을 리턴하고, 키가 존재하지 않으면 `null` 리턴함
- `HashSet`에서는 모든 `value`가 `PRESENT`로 동일하므로, `map.put()`이 `PRESENT`를 리턴하면 동일한 키가 존재하는 함을 의미

### 🧮 3-2) 해시
1. 객체의 `hashCode()` 호출
2. 해시 보완 처리
3. 버킷 인덱스 계산
   ```
   index = (n - 1) & hash
   ```
    - `n`은 버킷 배열의 크기 (항상 2의 거듭제곱)
    - `hash`는 계산된 해시값

### ⚡ 3-3) `putVal()` 동작
`HashSet`의 `add()` 메서드는 `map.put()`을 호출하고, 내부적으로 `HashMap`의 `putVal()` 메서드를 호출함

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 1. 테이블 초기화 검사
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;

    // 2. 버킷 위치 계산 및 빈 버킷 처리
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        // 3. 중복 키 검사
        if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        // 4. 트리 노드 처리
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        // 5. 연결 리스트 처리
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1)
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        // 6. 기존 값 갱신
        if (e != null) {
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    // 7. 크기 관리
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```

- 테이블 초기화 검사 : 테이블이 비어있으면 `resize()`로 초기화. 기본 초기 크기는 16
- 버킷 위치 계산 : `(n-1) & hash` 연산으로 인덱스 계산. 비어있는 버킷이면 새 노드 직접 삽입
- 키 중복 검사 : 해시값 비교 → 레퍼런스 동일성 검사 → `equals()`로 내용 비교
- 트리 노드 처리 : Red-Black 트리 구조 활용, 로그 시간 복잡도 보장
- 연결 리스트 처리 : 순차 탐색으로 중복 검사. 필요시 트리로 변환

## 🛡️ 4. 충돌 해결 방식

### 4-1) 연결 리스트 (Separate Chaining)
- 동일한 버킷에 여러 노드를 연결 리스트로 저장
- 간단하지만 길이가 길어질 수 있는 단점 존재

### 4-2) 트리화 (Tree Conversion)
- 버킷의 크기가 특정 임계값을 넘으면 Red-Black 트리로 변환
- `TREEIFY_THRESHOLD = 8` (트리화 임계값)
- `UNTREEIFY_THRESHOLD = 6` (역트리화 임계값)
- 최소 버킷 배열 크기 = 64

## ⚡ 5. 성능 최적화

### 5-1) 로드 팩터 (Load Factor)
- `static final float DEFAULT_LOAD_FACTOR = 0.75f`
- 공간과 시간의 트레이드오프 존재
    - 낮은 값 : 빠른 검색, 많은 메모리 사용
    - 높은 값 : 적은 메모리 사용, 느린 검색

### 5-2) 리사이징 (Resizing)
```java
final Node<K,V>[] resize() {
    // 새 크기 계산
    // 모든 요소 재배치
    // 임계값 조정
}
```

- 주요 변수
    - `Node<K,V>[] oldTab = table;` : 현재 테이블
    - `int oldCap = (oldTab == null) ? 0 : oldTab.length;` : 현재 테이블 크기
    - `int oldThr = threshold;` : 현재 임계값
    - `int newCap, newThr = 0;` : 새로운 크기와 임계값

#### 1. 새로운 크기 결정
1-1) 기존 테이블이 있는 경우 (`oldCap > 0`)
```java
if (oldCap > 0) {
    if (oldCap >= MAXIMUM_CAPACITY) {
        threshold = Integer.MAX_VALUE;
        return oldTab;
    } else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
            oldCap >= DEFAULT_INITIAL_CAPACITY)
        newThr = oldThr << 1;
}
```

1-2) 초기 생성 시 (`oldCap == 0`)
```java
else if (oldThr > 0)
    newCap = oldThr;
else {
    newCap = DEFAULT_INITIAL_CAPACITY;
    newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
}
```

#### 2. 데이터 재배치
```java
if (e.next == null) // 단일 노드 처리
    newTab[e.hash & (newCap - 1)] = e;
else if (e instanceof HashMap.TreeNode) // 트리 노드 처리
    ((HashMap.TreeNode<K,V>)e).split(this, newTab, j, oldCap);
else { // 링크드 리스트 처리
    HashMap.Node<K,V> loHead = null, loTail = null; // 기존 인덱스
    HashMap.Node<K,V> hiHead = null, hiTail = null; // 새 인덱스
    HashMap.Node<K,V> next;
    do { ... // 노드 분할 로직
```

## ⚖️ 6. HashSet과 HashMap 비교

| **HashSet**                 | **HashMap**                |
|-----------------------------|----------------------------|
| `Set` 인터페이스 구현      | `Map` 인터페이스 구현     |
| 객체(`Objects`) 저장 대상 | key - value 쌍 저장       |
| 비교적 느림                | 비교적 빠름               |

# 알고리즘 성능 비교: O(n) vs O(log n)
## 📚 실생활 예시: 숫자 카드 찾기 게임
### 🎯 문제 상황
1부터 1,000,000까지의 숫자 카드에서 상대방이 생각하는 카드의 위치 찾기
### 📊 두 가지 시나리오 비교
#### 1️⃣ O(n) - 무작위로 섞인 카드에서 찾기

초기 조건: 1부터 1,000,000까지의 숫자 카드를 무작위로 섞음
검색 방법: 카드를 한 장씩 순차적으로 확인
제약 사항: 정렬되어 있지 않아 중간값 확인이 의미 없음
최악의 경우: 찾는 카드가 맨 마지막에 있다면 1,000,000번의 확인 필요
```
[5, 2, 9, 1, 7, ..., 찾는 숫자]
↓
모든 카드를 하나씩 확인해야 함
```

#### 2️⃣ O(log n) - 정렬된 카드에서 찾기

초기 조건: 1부터 1,000,000까지의 숫자 카드가 순서대로 정렬
검색 방법: 중간 지점을 확인하여 검색 범위를 절반으로 줄임
장점: 매 시도마다 확인이 필요한 카드의 수가 절반으로 감소
최악의 경우: 약 20번(log₂1,000,000 ≈ 20)의 확인으로 충분
```
[1, 2, 3, ..., 500000, 500001, ..., 1000000]
                  ↓
중간값 확인 후 절반의 범위만 탐색
```

## 💡 성능 비교
알고리즘|최악의 경우 연산 횟수 | 시간 복잡도 |
|-------|-------------------|------------|
순차 검색|  1,000,000번 | O(n), 선형 시간 |
이진 검색| 약 20번 | O(log n), 로그 시간|
<details>
    <summary>성능 비교 테스트 코드</summary>

```java
public class PerformanceComparison {
    private static final int DATA_SIZE = 1_000_000;
    private static final int SEARCH_ITERATIONS = 1000;

    public static void main(String[] args) {
        // 테스트 데이터 생성
        List<Integer> data = generateData();
        Random random = new Random();

        // O(n) 검색 성능 측정
        long linearStartTime = System.nanoTime();
        int linearCnt = 0;

        for (int i = 0; i < SEARCH_ITERATIONS; i++) {
            int target = random.nextInt(DATA_SIZE);
            linearCnt += linearSearch(data, target);
        }

        long linearEndTime = System.nanoTime();
        double linearAvgTime = (linearEndTime - linearStartTime) / (double) SEARCH_ITERATIONS;

        // O(log n) 검색 성능 측정
        Collections.sort(data);  // 이진 검색을 위한 정렬
        long binaryStartTime = System.nanoTime();
        int binaryCnt = 0;

        for (int i = 0; i < SEARCH_ITERATIONS; i++) {
            int target = random.nextInt(DATA_SIZE);
            binaryCnt += binarySearch(data, target);
        }

        long binaryEndTime = System.nanoTime();
        double binaryAvgTime = (binaryEndTime - binaryStartTime) / (double) SEARCH_ITERATIONS;

        // 결과 출력
        System.out.println("=== 성능 비교 결과 ===");
        System.out.println("데이터 크기: " + DATA_SIZE);
        System.out.println("검색 반복 횟수: " + SEARCH_ITERATIONS);
        System.out.println("\n1. 선형 검색 (O(n)):");
        System.out.printf("   평균 수행 시간: %.2f ns\n", linearAvgTime);
        System.out.printf("   평균 연산 횟수: %d\n", linearCnt / SEARCH_ITERATIONS);
        System.out.println("\n2. 이진 검색 (O(log n)):");
        System.out.printf("   평균 수행 시간: %.2f ns\n", binaryAvgTime);
        System.out.printf("   평균 연산 횟수: %d\n", binaryCnt / SEARCH_ITERATIONS);
    }

    private static List<Integer> generateData() {
        List<Integer> data = IntStream.rangeClosed(1,DATA_SIZE)
                .boxed()
                .collect(Collectors.toList());
        Collections.shuffle(data);
        return data;
    }

    private static int linearSearch(List<Integer> data, int target) {
        int cnt = 0;
        for (int num : data) {
            cnt++;
            if (num == target) {
                break;
            }
        }
        return cnt;
    }

    private static int binarySearch(List<Integer> data, int target) {
        int cnt = 0;
        int left = 0;
        int right = data.size() - 1;

        while (left <= right) {
            cnt++;
            int mid = left + (right - left) / 2;
            int midVal = data.get(mid);

            if (midVal == target) {
                break;
            }

            if (midVal < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return cnt;
    }
}

```

</details>
<img width="294" alt="image" src="https://github.com/user-attachments/assets/ef84bf19-c6fc-40c8-a8eb-4b334f463a13" />


## 🔑 핵심 차이점

O(n): 데이터가 2배가 되면 검색 시간도 2배로 증가
O(log n): 데이터가 2배가 되어도 검색 시간은 1회만 증가

## 📈 결론
정렬된 데이터에서의 이진 검색(O(log n))은 순차 검색(O(n))에 비해 압도적으로 효율적이다. 100만 개의 큰 데이터에서도 최대 20번의 연산만으로 특정 값을 찾을 수 있다.
