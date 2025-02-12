# 웹 서버와 WAS 서버

| 항목       | 웹 서버 | WAS 서버 |
|------------|-----------------------------|------------------------------------------------|
| **정의**   | 정적인 콘텐츠(HTML, CSS, 이미지 등)를 제공하는 서버 | 동적인 콘텐츠(웹 애플리케이션)를 처리하고 제공하는 서버 |
| **기능**   | HTTP 프로토콜을 이용해 클라이언트에게 웹 페이지 제공 | 웹 애플리케이션 실행 및 데이터 처리, 웹 서버와 클라이언트 간의 중계 역할 |
| **주요 소프트웨어** | Apache, Nginx, IIS | Tomcat, JBoss, WebLogic, WebSphere |

## 📌 웹 서버 (Web Server)

웹 서버는 클라이언트가 웹 브라우저를 통해 요청한 **정적 콘텐츠**를 제공하는 역할을 합니다.

> 웹 서버는 주로 **HTTP 프로토콜**을 사용하여 작동하며,  
> 클라이언트가 URL을 통해 요청한 웹 페이지를 찾아 전송해줍니다.

## 🚀 WAS 서버 (Web Application Server)
WAS 서버는 웹 애플리케이션을 실행하여 **동적 콘텐츠**를 생성하고,  
웹 서버와 클라이언트 간의 데이터 처리를 담당하는 역할을 합니다.

> WAS 서버는 클라이언트의 요청에 따라 **데이터베이스에서 정보를 가져오거나**,  
> 웹 애플리케이션을 실행하여 **동적인 웹 페이지**를 생성한 후 결과를 웹 서버에 전달합니다.  
> 웹 서버는 이를 받아 클라이언트에게 전달합니다.
---
# Spring Boot에서 사용되는 다양한 Bean 

### 1. **`@Component` 계열 어노테이션 (`@Component`, `@Service`, `@Repository`, `@Controller`)**

### 설명:

- `@Component`: 기본적인 Spring Bean 등록 어노테이션
- `@Service`: 서비스 계층을 나타낼 때 사용
- `@Repository`: DAO(Data Access Object) 계층을 나타낼 때 사용, 예외 변환(AOP) 기능 제공
- `@Controller`: Spring MVC 컨트롤러를 나타낼 때 사용

### 장점:

- 간결한 선언 방식 (`@Component` 계열 어노테이션만 추가하면 됨)
- `@ComponentScan`을 사용하면 패키지 단위로 자동 스캔이 가능

### 단점:

- 특정 설정이 필요한 경우 (예: 외부 라이브러리 객체 등록)에는 사용이 어려움
- Bean 이름을 변경하려면 따로 명시해야 함 (`@Component("beanName")`)


### 2. **`@Bean`을 사용한 수동 등록**

### 설명:

- `@Configuration` 클래스 내부에서 `@Bean` 어노테이션을 사용하여 직접 Bean을 생성

```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

### 장점:

- 외부 라이브러리나 직접 생성해야 하는 객체를 Bean으로 등록할 때 유용
- 생성 로직을 직접 제어할 수 있어 세밀한 설정이 가능

### 단점:

- `@ComponentScan`을 통한 자동 등록보다 코드가 길어질 수 있음
- 수동으로 모든 의존성을 관리해야 하므로 실수가 발생할 가능성이 있음


### 3. **XML 기반의 Bean 등록 (구식)**

### 설명:

- `applicationContext.xml` 또는 다른 XML 파일을 사용하여 `<bean>` 태그로 Bean을 등록하는 방식

```xml
<bean id="myService" class="com.example.MyService"/>
```

### 장점:

- XML을 사용하여 설정을 분리할 수 있어 환경에 따라 설정을 변경하기 쉬움
- Spring Boot의 Java 기반 설정이 등장하기 전에 사용되던 방식으로, 이전 프로젝트와의 호환성이 좋음

### 단점:

- XML 파일을 따로 관리해야 하므로 유지보수성이 떨어짐
- Java 코드와 설정이 분리되어 있어 직관성이 낮음


### 4. **FactoryBean을 이용한 등록**

### 설명:

- `FactoryBean<T>` 인터페이스를 구현하여 특정 객체를 생성하는 방식

```java
public class MyFactoryBean implements FactoryBean<MyService> {
    @Override
    public MyService getObject() {
        return new MyService();
    }

    @Override
    public Class<?> getObjectType() {
        return MyService.class;
    }
}
```

### 장점:

- 복잡한 Bean 생성 로직을 캡슐화할 수 있음
- Bean의 생성과 초기화 과정을 커스텀하게 설정 가능

### 단점:

- 일반적인 `@Component`나 `@Bean` 방식보다 사용이 어렵고 코드가 길어질 수 있음
- 특별한 경우에만 사용되므로 일반적인 Bean 등록 방식보다 활용도가 낮음


### 5. **`@Import`를 사용한 등록**

### 설명:

- 다른 설정 클래스나 특정 Bean을 포함하고 싶을 때 사용

```java
@Configuration
@Import(AppConfig.class)
public class MainConfig {}
```

### 장점

- 모듈화된 설정을 구성할 수 있어 유지보수성이 높음
- 여러 설정을 조합하여 유연한 Bean 등록이 가능

### 단점:

- `@ComponentScan`처럼 자동으로 스캔하는 방식이 아니므로 직접 설정을 추가해야 함
- 설정이 많아질 경우 관리가 어려울 수 있음


## **비교 정리**

| 등록 방식 | 장점 | 단점 |
| --- | --- | --- |
| `@Component` 계열 | 자동 스캔 가능, 간결한 선언 | 외부 라이브러리 등록이 어려움 |
| `@Bean` (Java Config) | 세밀한 설정 가능, 외부 객체 등록 용이 | 코드가 길어질 수 있음 |
| XML 설정 | 환경에 따라 설정 분리 가능 | 유지보수 어려움, 가독성 낮음 |
| `FactoryBean` | 복잡한 Bean 생성 캡슐화 가능 | 사용법이 어렵고 코드가 복잡 |
| `@Import` | 모듈화된 설정 구성 가능 | 직접 설정을 추가해야 해서 관리 필요 |
