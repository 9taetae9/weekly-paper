## 1️⃣ Spring Framework의 탄생 배경

- **2002년**, **로드 존슨(Rod Johnson)** 이 EJB(Enterprise JavaBeans)의 복잡성을 해결하기 위해 개발.
- **EJB의 주요 문제점**
    1. **무거운 실행 환경**: Java EE 서버가 필요하며 배포 및 실행 속도가 느림. 
    2. **높은 결합도**: JNDI 기반 객체 조회 방식으로 인해 유연성이 부족함. 
    3. **테스트 어려움**: 컨테이너 의존적이라 단위 테스트가 어렵고 유지보수가 복잡함.
    4. **AOP 미지원**: 로깅, 보안과 같은 **횡단 관심사**를 분리하기 어려워 유지보수 부담 증가.
    
    <aside>
    💡
    
    **횡단 관심사 :** 애플리케이션 전반에서 여러 모듈(또는 클래스)에 걸쳐 공통적으로 적용되는 기능
    
    </aside>
    

---

## 2️⃣ Spring Framework의 등장과 해결한 문제들

### ✅ 의존성 주입(DI, Dependency Injection)

- JNDI 없이 IoC 컨테이너를 이용하여 객체를 관리하여 **결합도를 낮춤**.

### ✅ 경량 컨테이너(Lightweight Container)

- Java EE 서버 없이 실행 가능하며, XML 설정뿐만 아니라 Java 코드와 어노테이션 기반 설정도 지원.

### ✅ AOP(관점 지향 프로그래밍) 지원

- 로깅, 트랜잭션 관리, 보안 등의 **횡단 관심사**를 분리하여 코드 중복을 줄이고 유지보수를 용이하게 함.

### ✅ POJO(Plain Old Java Object) 기반 개발

- 특정 컨테이너(EJB)에 의존하지 않고 **일반 Java 객체만으로 개발 가능**, 단위 테스트와 통합 테스트가 용이함.

### ✅ 모듈화(Modularization)

- Spring MVC, Spring Security, Spring Boot 등 **필요한 기능만 선택적으로 사용 가능**하여 개발 유연성이 높음.

---

# 📌 EJB vs Spring 컨테이너 비교

| 구분 | **EJB 컨테이너** | **Spring 컨테이너** |
| --- | --- | --- |
| **실행 환경** | Java EE 서버(WebLogic, JBoss 등) 필요 | Java SE에서도 실행 가능 |
| **배포 방식** | EAR 또는 WAR 패키징 필요 | JAR 파일로 단순 실행 가능 |
| **객체 관리** | JNDI 기반 객체 조회 | IoC 컨테이너를 통한 DI 지원 |
| **트랜잭션** | 컨테이너 자동 관리 | AOP 기반 선언적 트랜잭션 지원 |
| **테스트 용이성** | 컨테이너 환경 필요, 단위 테스트 어려움 | POJO 기반, 단위 테스트 용이 |

---

# 📌 **프레임워크와 라이브러리의 차이**

| 구분 | **프레임워크 (Framework)** | **라이브러리 (Library)** |
| --- | --- | --- |
| **제어 흐름** | IoC(Inversion of Control) 적용, 실행 흐름을 프레임워크가 관리 | 개발자가 직접 호출하여 실행 |
| **사용 방식** | 프레임워크의 구조를 따라야 함 | 필요한 기능만 선택하여 사용 가능 |
| **확장성** | 규칙을 따라야 하지만 다양한 기능 제공 | 특정 기능만 제공 |

---

## 📌 **예제 비교**

### 1️⃣ **라이브러리 사용 예시 (`Math` 클래스)**

Java의 `Math` 클래스는 단순히 필요한 기능을 제공하는 라이브러리이며, 개발자가 직접 메서드를 호출해야 합니다.

```java
public class LibraryExample {
    public static void main(String[] args) {
        double number = -25;
        double absoluteValue = Math.abs(number);  // 개발자가 직접 Math의 메서드를 호출
        System.out.println("Absolute Value: " + absoluteValue);
    }
}
```

✅ **제어 흐름을 개발자가 직접 관리** → `Math.abs(number)`를 명시적으로 호출해야 함.

✅ **단순한 기능 제공** → `Math`는 수학 연산만 수행하며, 실행 흐름을 제어하지 않음.

---

### 2️⃣ **프레임워크 사용 예시 (Spring Framework)**

Spring은 실행 흐름을 직접 관리하며, 개발자는 프레임워크의 규칙을 따라야 합니다.

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class FrameworkExample {
    public static void main(String[] args) {
        // IoC 컨테이너에서 Bean을 자동으로 관리
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        MyService myService = context.getBean(MyService.class);
        myService.execute();
    }
}
```

✅ **Spring이 실행 흐름을 제어** → `ApplicationContext`가 객체를 생성 및 관리.

✅ **개발자는 객체를 직접 생성하지 않음** → `new MyService()` 없이 Spring이 주입.
