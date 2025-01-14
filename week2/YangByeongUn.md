### **1. SRP (Single Responsibility Principle, 단일 책임 원칙)**

**정의**: 하나의 클래스는 하나의 책임만 가져야 한다.

**목적**: 클래스 변경의 이유를 하나로 제한하여 코드의 응집도를 높이고 변경의 영향을 최소화.

<aside>

Repository 클래스가 객체의 조회, 생성, 삭제 작업을 모두 처리하는 것은 **"데이터 관리"**라는 하나의 책임으로 간주할 수 있다면 SRP를 위반하지 않습니다. 하지만 비즈니스 로직이나 다른 데이터 소스와의 상호작용 등 **여러 책임이 섞이는 경우**에는 SRP를 위반하게 됩니다.

</aside>

```
public class UserRepository {
	private readonly DbContext _context;
	
	public UserRepository(DbContext context) {
	    _context = context;
	}
	
	public User GetUserById(int id) {
	    var user = _context.Users.FirstOrDefault(u => u.Id == id);
	    if (user == null) {
	        throw new Exception("User not found");
	    }
	    return user;
	}
	
	public void AddUser(User user) {
	    if (user.Age < 18) {
	        throw new Exception("User must be at least 18 years old");
	    }
	    //나이 검증과 예외 처리는 데이터 접근 책임을 넘어섬.
	    //해결 방법: 나이 검증은 Service 계층으로 이동.
	    _context.Users.Add(user);
	    _context.SaveChanges();
	}
}
```

### **2. OCP (Open/Closed Principle, 개방/폐쇄 원칙)**

**정의**: 소프트웨어 엔티티는 확장에 열려 있고, 수정에는 닫혀 있어야 한다.

**목적**: 기존 코드를 변경하지 않고 기능을 확장할 수 있도록 함.

```java
//OCP를 위반한 코드
public class DiscountCalculator {
    public double calculateDiscount(String customerType, double amount) {
        if ("Regular".equals(customerType)) {
            return amount * 0.9; // 10% 할인
        } else if ("VIP".equals(customerType)) {
            return amount * 0.8; // 20% 할인
        }
        return amount; // 할인 없음
    }

    public static void main(String[] args) {
        DiscountCalculator calculator = new DiscountCalculator();
        System.out.println(calculator.calculateDiscount("VIP", 100)); // 출력: 80.0
    }
}

```

- 새로운 고객 유형으로 “Gold” 등급이 추가되었다면 기존 코드를 수정해야 한다.

```
public interface DiscountPolicy {
    double applyDiscount(double amount);
}

public class RegularDiscountPolicy implements DiscountPolicy {
    @Override
    public double applyDiscount(double amount) {
        return amount * 0.9; // 10% 할인
    }
}

public class VIPDiscountPolicy implements DiscountPolicy {
    @Override
    public double applyDiscount(double amount) {
        return amount * 0.8; // 20% 할인
    }
}

//이 부분만 추가하며 확장 가능
public class GoldDiscountPolicy implements DiscountPolicy {
    @Override
    public double applyDiscount(double amount) {
        return amount * 0.7; // 30% 할인
    }
}

public class DiscountCalculator {
    private DiscountPolicy discountPolicy;

    public DiscountCalculator(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }

    public double calculateDiscount(double amount) {
        return discountPolicy.applyDiscount(amount);
    }

    public static void main(String[] args) {
        // Regular 고객
        DiscountCalculator regularCalculator = new DiscountCalculator(new RegularDiscountPolicy());
        System.out.println(regularCalculator.calculateDiscount(100)); // 출력: 90.0

        // VIP 고객
        DiscountCalculator vipCalculator = new DiscountCalculator(new VIPDiscountPolicy());
        System.out.println(vipCalculator.calculateDiscount(100)); // 출력: 80.0

        // Gold 고객
        DiscountCalculator goldCalculator = new DiscountCalculator(new GoldDiscountPolicy());
        System.out.println(goldCalculator.calculateDiscount(100)); // 출력: 70.0
    }
}

```


### `map()`

- **역할**: 데이터를 변환하여 새로운 형태로 변환.
- **예시**:
    
    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Christine");
    names.stream()
         .map(String::toUpperCase)
         .forEach(System.out::println);
    
    ```
    
- **출력**:
```java
ALICE
BOB
CHRISTINE
```
### `flatMap()`

- **역할**: 중첩된 구조를 단일 구조로 펼침.
- **예시**:
    
    ```java
    List<List<String>> nestedList = Arrays.asList(
        Arrays.asList("apple", "banana"),
        Arrays.asList("cherry", "mango")
    );
    nestedList.stream()
              .flatMap(Collection::stream)
              .forEach(System.out::println);
    
    ```
    
- **출력**:
    
    ```
    apple
    banana
    cherry
    mango
    
    ```

### 예제: 3중 중첩된 리스트

### 중첩 리스트

```java
java
코드 복사
List<List<List<String>>> nestedList = Arrays.asList(
    Arrays.asList(Arrays.asList("A", "B"), Arrays.asList("C", "D")),
    Arrays.asList(Arrays.asList("E", "F"), Arrays.asList("G", "H"))
);

```

### 1. `flatMap()`으로 1단계만 평탄화

```java
java
코드 복사
List<List<String>> flatList = nestedList.stream()
                                        .flatMap(List::stream) // 1단계 평탄화
                                        .collect(Collectors.toList());

System.out.println(flatList);
// 출력: [[A, B], [C, D], [E, F], [G, H]]

```

**설명**:

- 최상위 리스트의 각 요소가 2중 리스트(`List<List<String>>`)로 변환됩니다.
- 아직 내부 리스트는 그대로 유지됩니다.


### flatMap() 과 Map() 차이

```java
List<String> words = Arrays.asList("Hello", "World");
List<String[]> result = words.stream()
                             .map(word -> word.split("")) // 문자열을 문자 배열로 변환
                             .collect(Collectors.toList());
System.out.println(result); // 출력: [[H, e, l, l, o], [W, o, r, l, d]]

List<String> result = words.stream()
                           .flatMap(word -> Arrays.stream(word.split(""))) // 배열을 평탄화
                           .collect(Collectors.toList());
System.out.println(result); // 출력: [H, e, l, l, o, W, o, r, l, d]

```

**차이점** :

- `map()`은 변환 결과를 유지 (중첩 구조).
- `flatMap()`은 변환 결과를 병합 (평탄화된 구조).
