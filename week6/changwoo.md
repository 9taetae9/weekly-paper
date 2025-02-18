### AOP란
- **AOP (Aspect-Oriented Programming, 관점 지향 프로그래밍)** 은 관점을 기준으로 코드를 모듈화하는 개발 방식입니다.
  > 즉, 하나의 기능을 핵심 기능과 부가 기능으로 구분하여 각각 하나의 관점으로 바라보는 개념입니다.
  - 핵심 기능: 비즈니스 로직 (예: 주문 처리, 결제 등)
  - 부가 기능: 로깅, 트랜잭션 관리, 보안, 예외 처리 등
  - AOP는 이러한 부가 기능을 비즈니스 로직에서 분리하여 코드의 가독성 및 유지보수성을 향상시킵니다.
### AOP가 필요한 이유
✅ 코드 중복 제거
- 로깅, 트랜잭션 관리, 보안, 예외 처리 등 반복적인 기능을 분리하여 중복을 제거할 수 있습니다.

✅ 관심사의 분리 (Separation of Concerns)
- 핵심 비즈니스 로직과 부가적인 기능을 분리하여 가독성과 재사용성을 높일 수 있습니다.

✅ OOP의 한계 극복
- 객체지향 프로그래밍(OOP)만으로는 모든 관심사를 효과적으로 모듈화하기 어려울 수 있습니다.
AOP를 사용하면 특정 기능을 따로 관리하여 코드 구조를 단순화할 수 있습니다.

✅ 유지보수 용이
- 공통 기능을 변경할 때 여러 클래스를 수정하는 대신, AOP의 한 부분만 변경하면 되므로 유지보수가 쉬워집니다.

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
   
     - 각 메서드의 실행 시간을 측정하는 로직을 한 곳에서 일괄 관리할 수 있습니다.
       
     - 비즈니스 로직 코드에서 불필요한 중복을 제거할 수 있습니다.
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
    
    - 트랜잭션 관리를 AOP로 분리하면 서비스 코드에서 @Transactional 어노테이션을 반복해서 작성할 필요가 없습니다.
    
    - 트랜잭션 관련 설정을 한 곳에서 통합 관리할 수 있어 유지보수성이 향상됩니다.
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

    - 보안 관련 로직이 비즈니스 코드에서 완전히 분리되어 가독성이 좋아집니다.
    
    - 보안 정책 변경 시 AOP 코드만 수정하면 전체 시스템에 적용됩니다.

### 결론

> Spring AOP는 로깅, 트랜잭션 관리, 보안, 예외 처리 등의 반복적인 기능을 비즈니스 로직에서 분리하여 코드의 가독성과 유지보수성을 높이는 강력한 도구입니다.
실제 애플리케이션 개발에서 AOP를 적절히 활용하면 중복 코드 제거, 관심사 분리, 유지보수성 향상 등의 효과를 얻을 수 있습니다. 🚀

---
     
