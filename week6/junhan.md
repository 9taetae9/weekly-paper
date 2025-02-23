## 🇶 Spring에서 AOP(Aspect Oriented Programming)가 필요한 이유와 이를 활용한 실제 애플리케이션 개발 사례에 대해 설명하세요.
### AOP란? 
- 여러 오브젝트에 나타나는 공통적인 부가 기능을 모듈화하여 재사용하는 기법
- 프로젝트 전반적으로 여러 메소드들에서 반복되는 코드 또는 로직을 모듈화 하여 재사용 할 수 있게 해주는 기능
### AOP를 사용하는 이유
- 코드의 재사용성과 중복제거
- 코드의 가독성, 유지보수성 및 생산성 증가
- 각 모듈에 수정이 필요하면 다른 모듈의 수정 없이 해당 로직만 변경
### AOP를 활용한 실제 애플리케이션 개발 사례
```java
@Aspect
@Component
public class DynamicAuthorizationAspect {
    @Autowired
    private SecurityService securityService;

    @Before("@annotation(RequiresPermission)")
    public void checkPermission(JoinPoint joinPoint) {
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        RequiresPermission annotation = signature.getMethod().getAnnotation(RequiresPermission.class);
        String requiredPermission = annotation.value();

        if (!securityService.hasPermission(requiredPermission)) {
            throw new AccessDeniedException("권한이 없습니다.");
        }
    }
}

```
- 위코드에ㅓ서 Aspect는 `@RequiresPermission` 어노테이션이 붙은 메소드 실행 전 사용자의 권한을 확인함
## 🇶 Spring MVC에서 클라이언트의 요청 처리 흐름을 @Controller와 @RestController의 차이점을 중심으로 각각의 처리 과정과 특징을 포함하여 설명하세오.

### @Controller
- 클라이언트 요청 → DispatcherServlet
- HandlerMapping이 적절한 컨트롤러 선택
- 컨트롤러가 요청 처리, Model에 데이터 담고 뷰 이름 반환
- ViewResolver가 뷰를 찾아 렌더링

### RestController 처리 흐름
- 클라이언트 요청 → DispatcherServlet
- HandlerMapping이 적절한 RestController 선택
- RestController가 요청 처리 및 객체 반환
- 객체를 JSON/XML로 변환하여 클라이언트에게 직접 반환

### @Controller와 @RestController의 차이점
| 특징 | @Controller | @RestController |
|------|-------------|-----------------|
| 반환 형식 | 뷰 | 데이터 객체 |
| 응답 생성 | ViewResolver 사용 | HttpMessageConverter 사용 |
| @ResponseBody | 필요 시 명시적 사용 | 자동 적용 |
| 주요 용도 | 전통적 웹 애플리케이션 | RESTful 웹 서비스 |


