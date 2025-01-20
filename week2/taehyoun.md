# 🌟 SRP & OCP

## 🧩 단일 책임 원칙 (Single Responsibility Principle, SRP) 

**정의:**
> 클래스는 변경할 이유가 단 하나여야 한다. 즉, 단 하나의 책임만 가져야 한다.

- **핵심 아이디어:** 각 모듈이나 클래스는 하나의 "행위자(Actor)"에 대해서만 책임져야 한다.
- **행위자:** 클래스의 변경을 요청할 수 있는 사람 또는 사용자 그룹

### "책임"의 이해 👀

| 책임 유형                | 범위                 | 적용 수준            | 목적                                | 결과                                   |
|--------------------------|----------------------|---------------------|-------------------------------------|---------------------------------------|
| 일반적인 책임             | 객체의 동작과 상태    | 메서드 또는 기능      | 객체의 역할 및 협력 정의             | 객체의 명확한 동작                    |
| SRP에 따른 책임           | 변경의 이유          | 클래스 또는 모듈     | 변경 범위의 관리와 통제             | 독립적으로 변경 가능한 모듈           |

### 🔑 SRP의 핵심 개념 
1. 책임 = 변경의 이유
2. 클래스는 하나의 행위자에 집중해야 한다.
3. 변경 이유가 다른 기능은 분리해야 한다.

### 🚨 SRP 위반 예제 
```java
public class Employee {
    public Money calculatePay(); // CFO의 책임 (급여 계산)
    public void save();          // CTO의 책임 (데이터 저장)
    public String reportHours(); // COO의 책임 (운영 보고)
}
```
- **문제점:**
  - `calculatePay`, `save`, `reportHours`가 서로 다른 행위자(CFO, CTO, COO)를 위한 기능을 제공
  - 하나의 메서드 변경이 다른 메서드에 영향을 줄 수 있음

### ✅ SRP 준수 예제 
```java
// CFO의 책임
public class EmployeePayCalculator {
    public Money calculatePay();
}

// CTO의 책임
public class EmployeeRepository {
    public void save();
}

// COO의 책임
public class EmployeeTimeReporter {
    public String reportHours();
}
```
- **장점:**
  - 변경 사항이 특정 클래스에만 영향을 미침
  - 책임 분리가 명확하여 유지보수 및 테스트가 용이함

### 추가 예제: 리포트 모듈 📝
#### SRP 위반
```java
class Report {
    private String content;
    private String format;

    public Report(String content, String format) {
        this.content = content;
        this.format = format;
    }

    public void generateReport() {
        System.out.println("Generating report data...");
        this.content = "Report data...";
    }

    public void formatReport() {
        System.out.println("Formatting report...");
    }

    public void printReport() {
        System.out.println("Printing report...");
    }
}
```
- **문제점:**
  - 데이터 생성, 포맷팅, 출력의 책임이 하나의 클래스에 집중.
  - 하나의 변경 사항이 전체 클래스에 영향을 미침.

#### SRP 준수
```java
// 1. 리포트 데이터 생성
class ReportGenerator {
    public String generateReport() {
        System.out.println("Generating report data...");
        return "Report data...";
    }
}

// 2. 리포트 포맷팅
class ReportFormatter {
    public String formatReport(String content, String format) {
        System.out.println("Formatting report...");
        return "Formatted " + content;
    }
}

// 3. 리포트 출력
class ReportPrinter {
    public void printReport(String formattedReport) {
        System.out.println("Printing report: " + formattedReport);
    }
}
```
- **장점:**
  - 데이터 생성, 포맷팅, 출력의 책임이 각각 독립적으로 분리
  - 각 클래스는 자신의 책임에만 집중

---

## 🚪 개방-폐쇄 원칙 (Open-Closed Principle, OCP) 

**정의:**
> 소프트웨어 엔티티(클래스, 모듈, 함수 등)는 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.

### OCP의 핵심 개념 💡
1. **확장에는 열려 있음:** 새로운 기능 추가 가능
2. **변경에는 닫혀 있음:** 기존 코드 수정 없이 확장 가능

### 장점 ✨
- **유지보수성 향상:** 새로운 기능 추가 시 기존 코드에 영향 없음
- **확장성 강화:** 핵심 코드를 변경하지 않고도 새로운 기능 구현 가능
- **테스트 용이성:** 기존 테스트가 유효성을 유지함

### 🔌 현대적인 OCP 구현: 플러그인 아키텍처 
#### 코어 시스템과 인터페이스
```java
// 코어 시스템 인터페이스
public interface Plugin {
    void execute();
}

// 플러그인 관리 코어 시스템
public class PluginSystem {
    private List<Plugin> plugins = new ArrayList<>();

    public void registerPlugin(Plugin plugin) {
        plugins.add(plugin);
    }

    public void runPlugins() {
        for (Plugin plugin : plugins) {
            plugin.execute();
        }
    }
}
```

#### 플러그인 구현
```java
// 플러그인 1
public class GreetingPlugin implements Plugin {
    @Override
    public void execute() {
        System.out.println("Hello, world!");
    }
}

// 플러그인 2
public class FarewellPlugin implements Plugin {
    @Override
    public void execute() {
        System.out.println("Goodbye, world!");
    }
}
```

#### 클라이언트 코드
```java
public class Main {
    public static void main(String[] args) {
        PluginSystem system = new PluginSystem();

        system.registerPlugin(new GreetingPlugin());
        system.registerPlugin(new FarewellPlugin());

        system.runPlugins();
    }
}
```

- **장점:**
  - `Plugin` 인터페이스를 구현하여 새로운 플러그인을 추가 가능
  - `PluginSystem`의 기존 코드는 변경되지 않음

---

### 📚 요약 

| 원칙  | 정의                                                        | 핵심 아이디어                            | 예제                                   |
|-------|------------------------------------------------------------|-----------------------------------------|---------------------------------------|
| SRP   | 클래스는 단 하나의 변경 이유만 가져야 한다.                  | 책임 = 변경의 이유                      | 급여 계산, 보고, 데이터 저장을 분리    |
| OCP   | 확장에는 열려 있고 변경에는 닫혀 있어야 한다.               | 기존 코드를 수정하지 않고 기능 확장 가능 | 플러그인 아키텍처                     |

이 원칙들을 준수함으로써 소프트웨어는 모듈화, 유지보수성, 테스트 용이성을 갖춘 구조로 설계됩니다. 🚀


# Java Stream API - map() & flatMap()

## 🛠 Stream API에서 map()과 flatMap()의 차이점

Stream API의 `map()`과 `flatMap()`은 데이터 변환과 처리에 사용되는 중요한 메서드입니다. 두 메서드 모두 스트림의 요소를 변환하지만, 처리 방식과 반환 결과에서 큰 차이가 있습니다:

| 메서드      | 설명                                                                                          | 반환 타입                  | 주요 특징                        |
|-------------|-----------------------------------------------------------------------------------------------|---------------------------|---------------------------------|
| `map()`     | 각 요소를 변환하여 1:1 매핑 수행. 변환된 요소를 새로운 스트림으로 반환                         | `Stream<R>`              | 요소 개수를 유지하며 변환       |
| `flatMap()` | 각 요소를 스트림으로 변환한 뒤, 중첩된 스트림 구조를 평면화하여 하나의 스트림으로 병합          | `Stream<R>`              | 중첩된 구조를 단일 레벨로 평탄화 |

---

## 1️⃣ map()

### 🧾 메서드 정의
```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```

- **입력 타입**: `T` (원본 스트림의 요소 타입)
- **출력 타입**: `R` (변환된 요소 타입)

### 🧩 주요 특징
- 각 요소를 독립적으로 변환합니다.
- 스트림의 요소 개수는 변하지 않습니다.
- 반환 타입은 변환된 요소의 타입입니다.

### 💻 예시 코드
```java
// 문자열의 길이로 변환
List<String> words = Arrays.asList("Java", "Stream", "API");
List<Integer> wordLengths = words.stream()
        .map(String::length)
        .collect(Collectors.toList());
// 결과: [4, 6, 3]

// 객체의 특정 필드 추출
List<User> users = getUserList();
List<String> userNames = users.stream()
        .map(User::getName)
        .collect(Collectors.toList());
```

---

## 2️⃣ flatMap()

### 🧾 메서드 정의
```java
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
```

- **입력 타입**: `T` (원본 스트림의 요소 타입)
- **출력 타입**: `R` (변환된 요소 타입)

### 🧩 주요 특징
- 각 요소를 0개 이상의 요소로 변환
- 중첩된 스트림 구조를 평탄화하여 단일 레벨의 스트림으로 병합
- 반환 타입은 평탄화된 스트림의 요소 타입

### 💻 예시 코드

### 중첩된 스트림 평탄화
```java
// 숫자 리스트의 리스트를 평탄화
List<List<Integer>> nestedNumbers = Arrays.asList(
        Arrays.asList(1, 2, 3),
        Arrays.asList(4, 5, 6),
        Arrays.asList(7, 8, 9)
);

List<Integer> flattenedNumbers = nestedNumbers.stream()
        .flatMap(List::stream)
        .collect(Collectors.toList());
// 결과: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### 입력값에 따라 유연한 개수 반환
```java
Stream<String> stream = Stream.of("a", "bb", "ccc");
Stream<String> result = stream.flatMap(str -> {
    // 문자열 길이에 따라 다른 개수 반환
    if (str.length() == 1) {
        return Stream.empty();  // 길이가 1이면 0개
    } else if (str.length() == 2) {
        return Stream.of(str);  // 길이가 2면 1개
    } else {
        return Stream.of(str, str.toUpperCase());  // 길이가 3이상이면 2개
    }
});
System.out.println(result.toList());
// 결과 : [bb, ccc, CCC]
// "a"는 0개, "bb"는 1개, "ccc"는 2개 반환
```
---

## 3️⃣ 실제 활용 사례

### 🛒 주문과 제품 처리

#### 예제 클래스
```java
class Order {
    private List<Product> products;

    public List<Product> getProducts() {
        return products;
    }
}

class Product {
    private String name;
    private double price;

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }
}
```

#### 💰 모든 주문의 총 금액 계산
```java
List<Order> orders = getOrderList();

// 총 금액 계산
double totalValue = orders.stream()
        .flatMap(order -> order.getProducts().stream())
        .mapToDouble(Product::getPrice)
        .sum();
```

#### 🛍️ 모든 제품 이름 추출
```java
// 제품 이름 추출 및 중복 제거
List<String> productNames = orders.stream()
        .flatMap(order -> order.getProducts().stream())
        .map(Product::getName)
        .distinct()
        .collect(Collectors.toList());
```

---

## 🔑 요약

- **`map()`**: 스트림의 각 요소를 변환하여 1:1 매핑을 수행
- **`flatMap()`**: 각 요소를 스트림으로 변환한 후, 중첩 구조를 평탄화하여 하나의 스트림으로 병합

### 📝 활용 포인트
- `map()`: 요소를 단순히 변환할 때 사용
- `flatMap()`: 중첩된 구조를 평탄화하고, 다수의 요소를 처리할 때 유용

---

