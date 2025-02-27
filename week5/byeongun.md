Here is the content from the file `week5/byeongun` in a normal text format:

# 웹 서버(Web Server)와 WAS(Web Application Server)의 차이 및 Spring Boot의 Bean 등록 방법

## 1. 웹 서버(Web Server)와 WAS(Web Application Server)의 차이
웹 서버와 웹 애플리케이션 서버(WAS)는 모두 클라이언트 요청을 처리하는 서버이지만, 그 역할과 기능이 다릅니다.

### 1) 웹 서버(Web Server)
- 정적인 콘텐츠(HTML, CSS, JavaScript, 이미지 등)를 클라이언트(브라우저)에 제공하는 서버입니다.
- 대표적인 웹 서버: **Apache HTTP Server, Nginx, Microsoft IIS**
- 요청을 처리하는 방식:
  - 클라이언트가 요청하면 정적인 파일을 그대로 반환하거나, 특정 경로의 요청을 WAS에 전달(Reverse Proxy 역할).
- 특징:
  - 빠른 정적 컨텐츠 제공
  - WAS와 연동하여 동적 컨텐츠 제공 가능 (Reverse Proxy)

### 2) 웹 애플리케이션 서버(WAS)
- 동적인 웹 애플리케이션을 실행하는 서버입니다.
- 주로 **Servlet, JSP, Spring 등의 애플리케이션을 실행**하며, 비즈니스 로직을 처리합니다.
- 대표적인 WAS: **Apache Tomcat, JBoss, WebSphere, WebLogic**
- 역할:
  - HTTP 요청을 받아 서블릿(Servlet) 또는 스프링(Spring) 애플리케이션에서 비즈니스 로직을 실행한 후 응답 생성
  - 데이터베이스 연동 및 트랜잭션 관리
  - 세션 관리 및 보안 기능 제공

### 3) 웹 서버와 WAS의 차이 정리

| 구분 | 웹 서버 (Web Server) | WAS (Web Application Server) |
|------|-----------------|------------------|
| 역할 | 정적 콘텐츠 제공 | 동적 웹 애플리케이션 실행 |
| 주요 기능 | HTML, CSS, JS, 이미지 제공, Reverse Proxy | 서블릿, JSP 실행, 트랜잭션 관리, 세션 관리 |
| 대표 제품 | Apache HTTP Server, Nginx | Tomcat, WebLogic, JBoss |

---

## 2. Spring Boot의 내장 톰캣은 웹 서버인가 WAS인가?
- **Spring Boot의 내장 톰캣(Tomcat)은 WAS(Web Application Server)에 해당합니다.**
- Tomcat 자체는 서블릿 컨테이너(Servlet Container)로, **웹 애플리케이션을 실행하는 기능**을 제공하므로 WAS로 볼 수 있습니다.
- 단, Apache HTTP Server 같은 별도의 웹 서버 없이 **내장 톰캣이 정적 리소스도 제공할 수 있으므로** 간단한 웹 서버 역할도 수행할 수 있습니다.

---

## 3. Spring Boot의 다양한 Bean 등록 방법과 비교
Spring Boot에서 빈(Bean)을 등록하는 방법은 크게 **세 가지**가 있습니다.

### 1) `@Component`, `@Service`, `@Repository` 등을 이용한 자동 등록
- Spring이 **클래스 경로(classpath)를 스캔하여 자동으로 Bean 등록**.
- `@Component`, `@Service`, `@Repository` 등의 어노테이션을 사용하면 자동 등록됨.

✅ **장점**
- 간단한 설정으로 자동 빈 등록 가능.
- Spring의 DI(Dependency Injection)와 쉽게 연동.

❌ **단점**
- 컴포넌트 스캔이 적용되는 패키지 안에서만 동작.
- 명확한 빈 관리를 위해 특정한 이름을 부여하기 어려움.

---

### 2) `@Bean`을 이용한 수동 등록
- `@Configuration` 클래스 내부에서 `@Bean`을 사용해 직접 Bean을 생성.

✅ **장점**
- 특정 객체를 생성하는 로직을 직접 관리할 수 있음.
- 같은 인터페이스의 여러 구현체를 등록하는 경우 유용.

❌ **단점**
- `@Component` 자동 등록보다 설정 코드가 많아짐.
- 수동으로 관리해야 하므로 실수로 빠뜨릴 가능성이 있음.

---

### 3) XML을 이용한 빈 등록 (이전 Spring 버전에서 사용)
- `applicationContext.xml`에서 `<bean>` 태그를 이용해 빈을 등록.

✅ **장점**
- Java 코드와 분리된 XML 설정이 가능.
- Spring을 지원하지 않는 환경에서도 사용 가능.

❌ **단점**
- XML 설정이 복잡하고 가독성이 떨어짐.
- 유지보수가 어려워 Spring Boot에서는 거의 사용되지 않음.

---

## 4. Bean 등록 방법 비교

| 방법 | 사용 어노테이션 | 장점 | 단점 |
|------|--------------|------|------|
| 자동 등록 (`@Component`, `@Service`, `@Repository`) | `@Component` 등 | 간단한 설정, 자동 등록 | 패키지 스캔 범위 제한 |
| 수동 등록 (`@Bean`) | `@Bean` | 생성 로직 직접 관리 가능 | 설정 코드 증가 |
| XML 등록 | `<bean>` 태그 | Java 코드와 분리 가능 | 복잡하고 유지보수 어려움 |

---

