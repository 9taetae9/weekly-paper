## Spring에서 AOP(Aspect Oriented Programming)가 필요한 이유와 이를 활용한 실제 애플리케이션 개발 사례에 대해 설명하세요.
### 🔹 **AOP(Aspect-Oriented Programming)란?**

AOP는 핵심 비즈니스 로직(Core Concerns)과 공통적인 부가 기능(Cross-Cutting Concerns)을 분리하여, 
코드의 **재사용성**과 **유지보수성**을 향상시키는 프로그래밍 기법

### 🔹 **AOP가 필요한 이유**

- **코드 중복 제거**: 로깅, 보안 검사, 트랜잭션 관리 등의 기능이 여러 클래스에서 반복적으로 사용될 수 있음 → AOP를 활용하면 한 곳에서 관리 가능
- **비즈니스 로직과 부가 기능의 분리**: 핵심 로직과 상관없는 공통 기능을 분리하여 가독성을 높임
- **유지보수 용이성**: 특정 부가 기능을 수정할 때 여러 클래스가 아닌 하나의 AOP 설정만 수정하면 됨

### 🔹 **Spring에서 AOP 활용 사례**

1. **로깅(Logging)**
    - 모든 서비스 메서드가 호출될 때 로그를 남기도록 AOP 적용
    
    ```java
    @Aspect
    @Component
    public class LoggingAspect {
        private static final Logger logger = LoggerFactory.getLogger(LoggingAspect.class);
    
        @Before("execution(* com.example.service.*.*(..))")
        public void logBefore(JoinPoint joinPoint) {
            logger.info("Executing: " + joinPoint.getSignature());
        }
    }
    
    ```
    
    - `@Before("execution(* com.example.service.*.*(..))")`→ `com.example.service` 패키지 내 모든 메서드 실행 전에 로깅 수행
    
2. **트랜잭션 관리(Transaction Management)**
    - `@Transactional`과 함께 AOP를 활용하여 메서드 실행 시 자동으로 트랜잭션 관리
      ```java
      @Aspect
        @Component
        public class TransactionAspect {
            @Around("@annotation(org.springframework.transaction.annotation.Transactional)")
            public Object manageTransaction(ProceedingJoinPoint joinPoint) throws Throwable {
                System.out.println("Transaction Start");
                Object result = joinPoint.proceed();
                System.out.println("Transaction Commit");
                return result;
            }
        }
      ```
## Spring MVC에서 클라이언트의 요청 처리 흐름을 @Controller와 @RestController의 차이점을 중심으로 각각의 처리 과정과 특징을 포함하여 설명하세오.
### 🔹 **@Controller와 @RestController 차이점**

| 구분 | @Controller | @RestController |
| --- | --- | --- |
| **주요 용도** | View 반환 (HTML, JSP 등) | JSON 또는 XML 데이터 반환 |
| **응답 처리** | `ModelAndView` 사용 | 객체를 JSON으로 직렬화하여 반환 |
| **내부 동작** | `return "viewName"` → ViewResolver가 뷰 찾음 | `@ResponseBody` 자동 포함, 데이터만 반환 |

### 🔹 **요청 처리 흐름 비교**

✅ **@Controller 흐름 (View 반환)**

1. 클라이언트가 URL 요청 → `DispatcherServlet`이 요청 받음
2. `HandlerMapping`이 해당 요청을 처리할 `@Controller`의 메서드 찾음
3. 메서드 실행 후 `return "viewName"`을 반환
4. `ViewResolver`가 뷰를 찾아서 HTML을 렌더링
5. 클라이언트에게 HTML 페이지 반환

```java
java
복사편집
@Controller
public class HomeController {
    @GetMapping("/home")
    public String home(Model model) {
        model.addAttribute("message", "Welcome to Spring!");
        return "home";  // home.jsp 또는 home.html 반환
    }
}

```

✅ **@RestController 흐름 (JSON 반환)**

1. 클라이언트가 URL 요청 → `DispatcherServlet`이 요청 받음
2. `HandlerMapping`이 해당 요청을 처리할 `@RestController`의 메서드 찾음
3. 메서드 실행 후 객체 반환
4. `HttpMessageConverter`가 객체를 JSON으로 변환
5. 클라이언트에게 JSON 응답 반환

```java
java
복사편집
@RestController
public class ApiController {
    @GetMapping("/api/message")
    public Map<String, String> getMessage() {
        return Map.of("message", "Hello, API!");  // JSON으로 변환됨
    }
}

```

### 🔹 **결론**

- `@Controller`는 HTML과 같은 뷰(View)를 반환할 때 사용
- `@RestController`는 JSON, XML 데이터를 반환할 때 사용 (`@ResponseBody` 포함)
- API 개발 시 `@RestController`를, 웹 애플리케이션 뷰를 다룰 때 `@Controller`를 사용
