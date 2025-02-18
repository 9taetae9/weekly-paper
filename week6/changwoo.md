### AOP란
- **AOP (Aspect-Oriented Programming, 관점 지향 프로그래밍)** 은 관점을 기준으로 코드를 모듈화하는 개발 방식
  > 즉, 하나의 기능을 핵심 기능과 부가 기능으로 구분하여 각각 하나의 관점으로 바라보는 개념
  - 핵심 기능: 비즈니스 로직 (예: 주문 처리, 결제 등)
  - 부가 기능: 로깅, 트랜잭션 관리, 보안, 예외 처리 등
  - AOP는 이러한 부가 기능을 비즈니스 로직에서 분리하여 코드의 가독성 및 유지보수성을 향상
### AOP가 필요한 이유
✅ 코드 중복 제거
- 로깅, 트랜잭션 관리, 보안, 예외 처리 등 반복적인 기능을 분리하여 중복을 제거할 수 있다.

✅ 관심사의 분리 (Separation of Concerns)
- 핵심 비즈니스 로직과 부가적인 기능을 분리하여 가독성과 재사용성을 높일 수 있다.

✅ OOP의 한계 극복
- 객체지향 프로그래밍(OOP)만으로는 모든 관심사를 효과적으로 모듈화하기 어려울 수 있다.
AOP를 사용하면 특정 기능을 따로 관리하여 코드 구조를 단순화할 수 있다.

✅ 유지보수 용이
- 공통 기능을 변경할 때 여러 클래스를 수정하는 대신, AOP의 한 부분만 변경하면 되므로 유지보수가 쉬워짐짐.

### Advice 종류

|어노테이션|설명|
|-------|---------------|
|@Before|메서드 실행 전에 동작하는 Advice|
|@After|메서드 실행 후에 동작하는 Advice|
|@AfterReturning|	메서드가 성공적으로 실행된 후 동작하는 Advice|
|@AfterThrowing|메서드 실행 중 예외 발생 후 동작하는 Advice|
|@Around|서드 실행 전후에 동작하며 실행을 직접 제어하는 Advice|

### AOP 활용 사례
1. 로깅(Logging) - 모든 서비스 메소드의 실행 시간을 기록
     ```
      import org.aspectj.lang.ProceedingJoinPoint;
      import org.aspectj.lang.annotation.Around;
      import org.aspectj.lang.annotation.Aspect;
      import org.springframework.stereotype.Component;

      @Aspect
      @Component
      public class LoggingAspect {

          @Around("execution(* com.example.service.*.*(..))") // 서비스 레이어의 모든 메서드 적용
          public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
              long start = System.currentTimeMillis();
              Object result = joinPoint.proceed(); // 실제 메서드 실행
              long end = System.currentTimeMillis();
      
              System.out.println(joinPoint.getSignature() + " 실행 시간: " + (end - start) + "ms");
              return result;
          }
      }
     ```
   
     ✅ 적용 효과
   
     - 각 메서드의 실행 시간을 측정하는 로직을 한 곳에서 일괄 관리할 수 있다.
       
     - 비즈니스 로직 코드에서 불필요한 중복을 제거할 수 있다.
2. 트랜잭션 관리 (Transaction Management) - 특정 메서드에서 예외 발생 시 자동으로 롤백되도록 설정
      ```
    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;
    import org.springframework.stereotype.Component;
    import org.springframework.transaction.annotation.Transactional;
    
    @Aspect
    @Component
    public class TransactionAspect {
    
        @Before("execution(* com.example.service.PaymentService.*(..))")
        @Transactional
        public void manageTransaction() {
            System.out.println("트랜잭션 시작");
        }
    }
    ```
    ✅ 적용 효과
    
    - 트랜잭션 관리를 AOP로 분리하면 서비스 코드에서 @Transactional 어노테이션을 반복해서 작성할 필요가 없다.
    
    - 트랜잭션 관련 설정을 한 곳에서 통합 관리할 수 있어 유지보수성이 향상된다.
3. 인증 및 권한 체크 (Security) - 사용자가 특정 기능을 실행하기 전에 인증이 필요한 경우, AOP를 활용하여 보안 로직을 일괄 적용

     ```
      import org.aspectj.lang.annotation.Aspect;
      import org.aspectj.lang.annotation.Before;
      import org.springframework.stereotype.Component;
      
      @Aspect
      @Component
      public class SecurityAspect {
      
          @Before("execution(* com.example.service.AdminService.*(..))")
          public void checkUserAuthentication() {
              System.out.println("사용자 인증 확인 중...");
              // 실제 인증 로직 (예: SecurityContextHolder에서 사용자 정보 확인)
          }
      }
     ```
     ✅ 적용 효과

    - 보안 관련 로직이 비즈니스 코드에서 완전히 분리되어 가독성이 좋아진다.
    
    - 보안 정책 변경 시 AOP 코드만 수정하면 전체 시스템에 적용된다.

### 결론

> Spring AOP는 로깅, 트랜잭션 관리, 보안, 예외 처리 등의 반복적인 기능을 비즈니스 로직에서 분리하여 코드의 가독성과 유지보수성을 높이는 강력한 도구
실제 애플리케이션 개발에서 AOP를 적절히 활용하면 중복 코드 제거, 관심사 분리, 유지보수성 향상 등의 효과를 얻을 수 있다. 🚀

---

## **Spring MVC에서 클라이언트 요청 처리 흐름**

Spring MVC에서 클라이언트 요청이 처리되는 과정은 `@Controller`와 `@RestController`의 사용 방식에 따라 다릅니다.  
두 어노테이션은 모두 **Spring MVC의 컨트롤러 역할**을 수행하지만, 응답 방식에 차이가 있습니다.

## **1. @Controller를 사용한 요청 처리 흐름**

`@Controller`는 **주로 뷰(View)를 반환**하는 경우에 사용됩니다.  
즉, **클라이언트가 HTML 페이지를 요청**하는 경우 적합합니다.

### **1️⃣ 요청 처리 과정**

1. 클라이언트가 **HTTP 요청**을 보냄 (`GET`, `POST`, `PUT`, `DELETE` 등)
2. `DispatcherServlet`이 요청을 받아 **HandlerMapping**을 통해 적절한 `@Controller`를 찾음
3. 매칭된 컨트롤러의 요청 처리 메서드가 실행됨
4. 해당 메서드는 **ModelAndView** 또는 **문자열(뷰 이름)**을 반환
5. `ViewResolver`가 반환된 뷰 이름을 사용하여 JSP, Thymeleaf 등의 뷰 파일을 찾아 렌더링
6. 클라이언트에게 최종 HTML 페이지를 응답

### **2️⃣ 예제 코드**

```
@Controller
@RequestMapping("/user")
public class UserController {

    @GetMapping("/profile")
    public String getUserProfile(Model model) {
        model.addAttribute("name", "김창우");
        return "userProfile"; // ViewResolver를 통해 userProfile.html 또는 userProfile.jsp를 찾음
    }
}
```
✅ 특징

- @Controller는 뷰(View) 페이지를 반환함
- Model 객체를 사용하여 데이터를 뷰에 전달 가능
- ViewResolver를 통해 JSP, Thymeleaf 등의 템플릿 엔진을 활용하여 HTML을 생성

## 2 @RestController를 사용한 요청 처리 흐름

@RestController는 JSON 또는 XML과 같은 데이터를 반환하는 컨트롤러입니다.
RESTful API 개발에 주로 사용되며, 클라이언트는 웹페이지가 아니라 JSON 등의 데이터를 요청합니다.

### **1️⃣ 요청 처리 과정**

1. 클라이언트가 **HTTP 요청**을 보냄 (`GET`, `POST`, `PUT`, `DELETE` 등)
2. DispatcherServlet이 요청을 받아 HandlerMapping을 통해 적절한 @RestController를 찾음
3. 매칭된 컨트롤러의 요청 처리 메서드가 실행됨
4. 해당 메서드는 ResponseBody를 포함하고 있어 데이터를 반환
5. HttpMessageConverter가 데이터를 JSON 또는 XML 등의 형식으로 변환
6. 클라이언트에게 JSON 또는 XML 데이터를 응답

### **2️⃣ 예제 코드**
```
@RestController
@RequestMapping("/api/user")
public class UserRestController {

    @GetMapping("/profile")
    public User getUserProfile() {
        return new User("김창우", 25, "developer");
    }
}
```
```
public class User {
    private String name;
    private int age;
    private String job;

    public User(String name, int age, String job) {
        this.name = name;
        this.age = age;
        this.job = job;
    }

    // Getter & Setter 생략
}
```
✅ 특징

- @RestController는 JSON 또는 XML 데이터를 반환함
- @ResponseBody를 포함하고 있어 별도로 명시할 필요 없음
- HttpMessageConverter를 사용하여 객체를 JSON으로 자동 변환

## 3 **@Controller** vs **@RestController** 비교
|구분|@Controller|@RestController|
|----|------------|-------------|
|응답 방식|뷰(View) 페이지 반환|JSON 또는 XML 데이터 반환|
|주요 용도|HTML 페이지 응답 (JSP, Thymeleaf 등)|RESTful API 개발 (JSON, XML 응답|
|데이터 처리 방식|	Model을 사용하여 뷰에 데이터 전달|HttpMessageConverter를 사용하여 JSON 변환|
|@ResponseBody 필요 여부|필요 (@ResponseBody를 메서드에 추가해야 JSON 반환)|필요 없음 (내장되어 있음)|
|뷰 템플릿 엔진 사용|O (JSP, Thymeleaf 등 활용)|X (JSON/XML 직접 반환)|

📌 @Controller

```
@Controller
@RequestMapping("/user")
public class UserController {

    @GetMapping("/info")
    public String getUserInfo(Model model) {
        model.addAttribute("name", "김창우");
        model.addAttribute("age", 25);
        return "userInfo"; // ViewResolver가 userInfo.html을 찾아 렌더링
    }
}
```
📌 클라이언트 요청 및 응답

- 요청: GET /user/info
- 응답: HTML 페이지 (userInfo.html 렌더링)

📌 @RestController 

```
@RestController
@RequestMapping("/api/user")
public class UserRestController {

    @GetMapping("/info")
    public Map<String, Object> getUserInfo() {
        Map<String, Object> response = new HashMap<>();
        response.put("name", "김창우");
        response.put("age", 25);
        return response; // JSON으로 반환됨
    }
}
```
📌 클라이언트 요청 및 응답

- 요청: GET /api/user/info
- 응답
```
{
    "name": "김창우",
    "age": 25
}
```

## 결론

✅ @Controller는 웹 애플리케이션의 뷰(HTML)를 반환할 때 사용

✅ @RestController는 RESTful API에서 JSON 또는 XML 데이터를 반환할 때 사용

✅ @RestController는 내부적으로 @ResponseBody를 포함하므로, 자동으로 JSON 변환

✅ @Controller는 ViewResolver를 통해 JSP, Thymeleaf 등과 함께 사용

💡 즉, 화면을 렌더링하는 경우 @Controller, REST API를 제공하는 경우 @RestController를 사용!

