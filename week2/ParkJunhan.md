# ✏️ Week2 위클리 페이퍼
## 🇶 객체지향 프로그래밍에서 '단일 책임 원칙(SRP)'과 '개방-폐쇄 원칙(OCP)'에 대해 설명하고, 각각의 원칙을 적용한 코드 예시를 들어주세요.
### 단일 책임 원칙 (Single Responsibility Principle, SRP)
- 단일 책임 원칙 (SRP)는 클래스는 단 하나의 책임만 가져야 한다는 원칙
- 각 클래스는 명확하고 잘 정의된 하나의 책임만을 가져야 함
- 클래스를 변경해야 하는 이유는 오직 하나여야 함
#### 단일 책임 원칙 (SRP)을 위반한 예시
``` java
class UserManager {
    public void createUser(String username, String password) {
        // 사용자 생성 로직
        System.out.println("User created: " + username);
        // 데이터베이스에 저장
        saveToDatabase(username, password);
        // 이메일 발송
        sendWelcomeEmail(username);
    }

    private void saveToDatabase(String username, String password) {
        // 데이터베이스 저장 로직
        System.out.println("Saved to database: " + username);
    }

    private void sendWelcomeEmail(String username) {
        // 이메일 발송 로직
        System.out.println("Welcome email sent to: " + username);
    }
}
```
- UserManager 클래스는 사용자 생성, 데이터베이스 저장, 이메일 발송 등 여러 책임을 가지고 있음
- UserManager 클래스는 사용자 생성 로직이 변경되거나, 데이터베이스 저장 방식이 변경되는 등 한 부분의 변경이 다른 부분에 영향을 미침
#### 단일 책임 원칙 (SRP)을 준수한 예시

```
class UserCreator {
    public void createUser(String username, String password) {
        // 사용자 생성 로직
        System.out.println("User created: " + username);
    }
}

class DatabaseManager {
    public void saveUser(String username, String password) {
        // 데이터베이스 저장 로직
        System.out.println("Saved to database: " + username);
    }
}

class EmailService {
    public void sendWelcomeEmail(String username) {
        // 이메일 발송 로직
        System.out.println("Welcome email sent to: " + username);
    }
}
```
- 단일 책임 원칙으로 사용자 생성, 데이터베이스 저장, 이메일 발송 등 기능을 클래스 별로 분리함
### 개방-폐쇄 원칙 (Open-Closed Principle, OCP)
- OCP는 소프트웨어 엔티티가 확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 한다는 원칙
- 확장에 열려 있다는 것은 새로운 기능을 추가할 때 기존 코드를 수정하지 않고도 가능해야 한다는 의미
- 수정에 닫혀 있다’는 것은 기존 코드의 변경 없이 동작을 확장하거나 변경할 수 있어야 한다는 의미
#### 개방-폐쇄 원칙 (Open-Closed Principle, OCP)을 위반한 예시코드
```
public class PaymentProcessor {
    public void processPayment(String paymentType, double amount) {
        if (paymentType.equals("CreditCard")) {
            System.out.println("신용카드로 " + amount + "원 결제 처리");
        } else if (paymentType.equals("PayPal")) {
            System.out.println("PayPal로 " + amount + "원 결제 처리");
        }
    }
}

```
- 위 코드는 새로운 결제 방식을 추가할 때마다 `PaymentProcessor` 클래스를 수정해야 하므로 OCP를 위반
#### 개방-폐쇄 원칙 (Open-Closed Principle, OCP)을 준수한 예시 코드
```
interface PaymentMethod {
    void processPayment(double amount);
}

class CreditCardPayment implements PaymentMethod {
    @Override
    public void processPayment(double amount) {
        System.out.println("신용카드로 " + amount + "원 결제 처리");
    }
}

class PayPalPayment implements PaymentMethod {
    @Override
    public void processPayment(double amount) {
        System.out.println("PayPal로 " + amount + "원 결제 처리");
    }
}

class PaymentProcessor {
    public void processPayment(PaymentMethod paymentMethod, double amount) {
        paymentMethod.processPayment(amount);
    }
}

// 새로운 결제 방식 추가
class BitcoinPayment implements PaymentMethod {
    @Override
    public void processPayment(double amount) {
        System.out.println("Bitcoin으로 " + amount + "원 결제 처리");
    }
}

```
- PaymentMethod` 인터페이스를 도입하여 새로운 결제 방식을 추가할 때 `PaymentProcessor` 클래스를 수정하지 않고도 시스템을 확장할 수 있게됨
- 새로운 결제 방식(예: Bitcoin)을 추가하려면 단순히 `PaymentMethod` 인터페이스를 구현하는 새로운 클래스를 만들면 됨

## 🇶 Stream API의 map과 flatMap의 차이점을 설명하고, 각각의 활용 사례를 예시 코드와 함께 설명해주세요.
### Map
- Map은 반복 가능한 배열을 대상으로, 배열의 각 요소를 하나하나 순회하며 그 요소를 조작하고, 조작한 요소들이 모여있는 배열을 리턴해 주는 것이다.
#### 특징:
	•	각 요소를 새로운 요소로 변환
	•	결과 스트림의 요소 개수는 원본 스트림과 동일
 ```
let exampleInt = [1, 2, 3]

let exampleString = exampleInt.map { value -> String in
    return String(value)
} 

// ["1", "2", "3"]
```
### flatMap
- flatMap은 스트림의 각 요소를 0개 이상의 요소를 포함하는 새로운 스트림으로 변환한 후, 이를 하나의 스트림으로 평면화함
#### 특징:
	•	각 요소를 스트림으로 변환 후 하나의 스트림으로 평면화
	•	결과 스트림의 요소 개수는 원본 스트림과 다를 수 있음
```
List<List<String>> nestedList = Arrays.asList(
    Arrays.asList("a", "b"),
    Arrays.asList("c", "d"),
    Arrays.asList("e", "f")
);

List<String> flattenedList = nestedList.stream()
                                       .flatMap(Collection::stream)
                                       .collect(Collectors.toList());
System.out.println(flattenedList); // 출력: [a, b, c, d, e, f]
```
### 차이점 
- map은 1:1 변환, flatMap은 1:N 변환
- map은 원본 구조 유지, flatMap은 중첩 구조를 평면화함




