### SRP
-   모든 클래스는 하나의 책임(기능)만 가지며, 클래스는 그 책임을 완전히 캡슐화 해야함
-   책임을 적절히 분배함으로 코드의 가독성 향상, 유지보수 용이, 다른 설계원리들을 적용하는 기초
-   메소드의 개수 ≠ 책임의 개수
-   SRP 원칙 적용 주의점
    -   클래스명은 책임의 소재를 알 수 있게 작명하는 것이 좋다
    -   결합도는 낮추고 응집도는 높이며 → 반대로 너무 많은 책임 분할로 인하여 책임이 여러군데 파편화 되어있는 경우에는 산탄총 수술로 다시 응집력을 높여주는 작업이 필요
        -   산탄총 수술
            -   부가 기능을 별개의 클래스로 분리해 책임을 담당
                -   여러 곳에 흩어진 공통 책임을 한 곳에 모으면서 응집도 높이기
                -   그러나 이런 독립 클래스를 구현하더라도 구현된 기능들을 호출하고 사용하는 코드는 해당 기능을 사용하는 코드 어딘가에 포함될 수밖에 없다.  
                  
---
​
### 예제 코드
-   SRP 적용 전
    
    ```
    public class Employee {
      private String name;
      private double salary;
    
      public Employee(String name, double salary) {
          this.name = name;
          this.salary = salary;
      }
    
      // 직원 정보 출력
      public void printDetails() {
          System.out.println("Name: " + name);
          System.out.println("Salary: " + salary);
      }
    
      // 세금 계산
      public double calculateTax() {
          return salary * 0.2; // 고정 세율 20%
      }
    
      // 급여 지급
      public void paySalary() {
          System.out.println("Paying salary of " + salary);
      }
    }
    ```
    
-   SRP 적용 후
    
    ```
    데이터 관리 클래스
    public class Employee {
      private String name;
      private double salary;
    
      public Employee(String name, double salary) {
          this.name = name;
          this.salary = salary;
      }
    
      public String getName() {
          return name;
      }
    
      public double getSalary() {
          return salary;
      }
    }
    ```
    
    ```
      // 세금 계산 클래스
      public class TaxCalculator {
              public double calculateTax(Employee employee) {
                  return employee.getSalary() * 0.2; // 고정 세율 20%
          }
      }
    
      // 급여 지급 클래스
      public class SalaryPayer {
          public void paySalary(Employee employee) {
              System.out.println("Paying salary of " + employee.getSalary() + " to " + employee.getName());
              }
      }
    
      // 출력 로직 처리 클래스
      public class EmployeePrinter {
          public void printDetails(Employee employee) {
                  System.out.println("Name: " + employee.getName());
              System.out.println("Salary: " + employee.getSalary());
              }    
      }
    ```
---
### OCP

-   기존의 코드를 변경하지 않으면서, 기능을 추가
-   확장에 대해서는 개방적(open), 수정에 대해서는 폐쇄적(closed)
-   DIP(Dependency Inversion Principle)과 밀접



-   OCP 위배

```
public class DiscountService {
    public double calculateDiscount(String customerType, double amount) {
        if (customerType.equals("Regular")) {
            return amount * 0.1; // Regular 고객: 10% 할인
        } else if (customerType.equals("VIP")) {
            return amount * 0.2; // VIP 고객: 20% 할인
        } else if (customerType.equals("New")) {
            return amount * 0.05; // 신규 고객: 5% 할인
        } else {
            return 0; // 할인 없음
        }
    }
}
```
> 새로운 고객 유형이 추가 될때 마다 메소드를 수정
- OCP 적용 후
  - 할인 정책 인터페이스 정의
     ```
     public interface DiscountPolicy {
        double calculateDiscount(double amount);
    }
    ```
    
  - 다양한 할인 정책
    ```
      // Regular 고객 할인 정책
      public class RegularDiscountPolicy implements DiscountPolicy {
          @Override
          public double calculateDiscount(double amount) {
              return amount * 0.1;
          }
      }

      // VIP 고객 할인 정책
      public class VIPDiscountPolicy implements DiscountPolicy {
          @Override
          public double calculateDiscount(double amount) {
              return amount * 0.2;
          }
      }

      // 신규 고객 할인 정책
      public class NewCustomerDiscountPolicy implements DiscountPolicy {
          @Override
          public double calculateDiscount(double amount) {
              return amount * 0.05;
          }
      }
     ```


>  새로운 고객 유형이 추가될 때 기존 코드를 수정하지 않고 새로운 DiscountPolicy 클래스를 추가하기만 하면 된다.

---
### map, flatMap
- map() : 각 요소를 변환한 스트림 반환
- flatMap(): 각 요소를 변환한 스트림들을 하나의 스트림으로 평면화하여 반환
- 코드 예시
  
  ```
    //map()=> 데이터를 변환해서 새로운 형태로 변환
    List<String> names=Arrays.asList("Alice","Bab","Christine");
    names.stream()
          .map(name->name.toUpperCase())
          .forEach(System.out::println);

  출력
  ALICE
  BAB
  CHRISTINE
  
  ```
  ```
     //flatMap() => 중첩구조를 단일 구조로 펼쳐
    List<List<String>> nestedList=Arrays.asList(
                    Arrays.asList("apple","banana"),
                    Arrays.asList("cherry","mango")
            );
    nestedList.stream().flatMap(Collection::stream).forEach(System.out::print);
    출력
    apple
    banana
    cherry
    mango
    ```
  - map() 사용이 적절한 경우
      - 단순 1:1 데이터 변환
      - 객체의 특정 필드 추출
      - 값 변환 작업
  - flaMap() 사용이 적절한 경우
      - 중첩된 컬렉션 처리
      - 복잡한 데이터 구조 평면화
      - 1:N관계의 데이터 처리  
