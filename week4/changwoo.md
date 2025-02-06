## 🇶 Spring Framework가 탄생하게 된 배경과 이를 통해 해결하고자 했던 문제점에 대해 설명하세요.
# 🌱 Spring Framework의 탄생 배경과 해결하려던 문제  

## 🔍 1. Spring Framework의 탄생 배경  
Spring Framework는 **Enterprise JavaBeans(EJB)의 복잡성과 무거움을 해결하기 위해** 등장 

### 📌 (1) EJB의 문제점  
EJB는 엔터프라이즈 애플리케이션을 개발하기 위해 Java EE 환경에서 제공되던 기술이지만, 다음과 같은 문제점이 있었습니다.  
- ❌ **복잡한 설정**: XML 설정이 많고, JNDI 조회 등 번거로운 작업이 필요함  
- ❌ **무거운 컨테이너**: 별도의 애플리케이션 서버(WebLogic, JBoss 등)에서 실행해야 함  
- ❌ **테스트 어려움**: 독립적인 단위 테스트가 어려움  

### 🚀 (2) Spring의 등장  
Spring은 **경량 프레임워크**로서 EJB의 복잡성을 해결하고자 등장했습니다.  

**Spring의 주요 특징**  
✅ **POJO(Plain Old Java Object) 기반 개발** → 일반 Java 객체만으로 개발 가능  
✅ **IoC(Inversion of Control) / DI(Dependency Injection) 지원** → 객체 생성을 컨테이너가 관리  
✅ **AOP(Aspect-Oriented Programming) 지원** → 로깅, 트랜잭션 관리 등 횡단 관심사 분리  
✅ **경량 컨테이너** → Tomcat 같은 서블릿 컨테이너에서도 실행 가능  

---


## 🇶 프레임워크와 라이브러리의 차이점을 제어 흐름의 주체와 사용 방식을 중심으로 설명하고, Spring Framework와 일반 Java 라이브러리를 예시로 들어 설명하세요.


## 🔍 2. 프레임워크와 라이브러리의 차이점  

| 구분 | 프레임워크 | 라이브러리 |
|------|------------|------------|
| **제어 흐름 (IoC)** | 제어의 흐름을 **프레임워크가 담당** | 개발자가 직접 라이브러리를 호출 |
| **사용 방식** | 특정 구조와 규칙을 따라야 함 | 필요한 기능을 가져다 사용 |
| **예시** | Spring Framework | Apache Commons Lang |

### 🎯 (3) 예시  
#### ✅ Spring Framework (프레임워크)  
- `@Component`, `@Service`, `@Repository` 등을 이용하여 빈을 등록하면, Spring이 자동으로 객체를 생성하고 주입하는 IoC 방식 적용
- 개발자가 `@Autowired`를 통해 필요한 빈을 주입받고 사용

#### ✅ Apache Commons Lang (라이브러리) -> 랜덤 문자열 생성
- `StringUtils.capitalize("hello")`와 같이 필요한 기능을 직접 호출해서 사용
- 제어 흐름을 강제하지 않으며, 개발자가 원하는 방식으로 호출 가능

