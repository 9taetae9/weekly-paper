📌 **객체지향 프로그래밍에서 '단일 책임 원칙(SRP)'과 '개방-폐쇄 원칙(OCP)'에 대해 설명하고, 각각의 원칙을 적용한 코드 예시를 들어주세요.**
## 단일 책임 원칙 (SRP)

**단일 책임 원칙** : "**객체는 단 하나의 책임만 가져야 한다**"는 원칙
- 하나의 클래스는 하나의 기능을 담당하며, 하나의 책임을 수행하는데 집중해야 한다.

### 책임의 기준 
- 단일 책임은 절대적인 개념이 아닌 상대적인 개념으로 기준이 모호할 수 있다.
- **클래스(모듈)을 변경하는 이유는 단 한가지여야 한다.**
```
  > **오해** : 메소드의 개수가 책임의 개수가 아니다.
  > - 하나의 모듈이 하나의 일만 해야 한다는 것이 아니다.
```

### 단일 책임 원칙을 사용해야 하는 이유
1. 특정 기능을 수정할 때 하나의 클래스만 수정하면 되기 떄문에 유지보수가 편리하다.
2. 기능 수정 시 발생할 수 있는 사이드 이펙트를 줄일 수 있다.
3. 단일 클래스의 기능이 적어져 종속성이 줄어든다.
4. 작고 잘 조직된 클래스는 검색과 이해가 쉽다.
5. 하나의 책임만 가지기때문에 구현 및 이해가 쉽다.

```
   > 단점 -> 기존 코드보다 길어질 수 있다.
   > 하지만 여러개의 클래스를 사용하는 것이 유지보수와 의미 파악 측면에서는 더 유리할 수 있다. 
```

### 예제 코드
``` java
//영화 정보 저장 클래스
class Movie {
  private String title;

  public Movie(String title) {
    this.title = title;
  }

  public String getTitle() {
    return title;
  }
}

//영화 출력 클래스
class MoviePrinter {
  public void printMovie(Movie movie) {
    System.out.println("Movie: " + movie.getTitle());
  }
}

//Main 클래스
public class Main {
    public static void main(String[] args) {
        Movie movie = new Movie("Harry Potter");
        MoviePrinter printer = new MoviePrinter();

        printer.printMovie(movie);
    }
}
```

## 개방 - 폐쇄 원칙 (OCP)
**개방-폐쇄 원칙** : 기존 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계되어야 한다는 원칙 
- 확장에 대해서는 **개방적(Open)**
- 수정에 대해서는 **폐쇄적(Closed)**

### 개방-폐쇄 원칙의 핵심
1. **확장**:
   - 새로운 기능을 추가할 수 있도록 열려 있어야 함
   - 모듈의 확장성을 보장
2. **폐쇄**:
   - 기존 코드를 직접 수정하지 않도록 설계
   - 유지보수 비용을 줄이고 유연하게 변경 사항을 적용할 수 있음

### 예제 코드

```java
public class MovieMain {
    public static void main(String[] args) {
        Movie[] movies = new Movie[4];
        movies[0] = new InsideOut();
        movies[1] = new HarryPotter();
        movies[2] = new Wicked();
        movies[3] = new Harbin();

        for (Movie movie : movies) {
            movie.screening();
        }
    }
}

class Movie {
    String title = "상영작";

    void screening() {
        System.out.println("현재 상영작은: " + title + "입니다.");
    }
}

class InsideOut extends Movie {
    public InsideOut() {
        title = "인사이드 아웃";
    }
}

class HarryPotter extends Movie {
    public HarryPotter() {
        title = "해리포터";
    }
}

class Wicked extends Movie {
    public Wicked() {
        title = "위키드";
    }
}

class Harbin extends Movie {
    public Harbin() {
        title = "하얼빈";
    }
}
```
---
📌 **Stream API의 map과 flatMap의 차이점을 설명하고, 각각의 활용 사례를 예시 코드와 함께 설명해주세요.**
## .map()
.map()은 단일 스트림의 원소를 매핑시킨 후 매핑시킨 값을 다시 스트림으로 반환하는 **중간 연산 담당**
<br> 즉, 객체에서 원하는 원소를 추출해내어 1:1 매핑 시키는 역할을 한다.

```java
//기본적인 map() 사용
List<String> names = Arrays.asList("Alice", "Charlie", "Gray");
List<Integer> nameLength = names.stream()
                .map(String::length)
                .collect(Collectors.toList());
System.out.println(nameLength); //출력결과 -> [5,7,4]
```

## .flatMap() : 스트림 평면화 
.flatMap() 은 Array나 Object 로 감싸져 있는 모든 원소를 단일 원소 스트림으로 변환시켜주는 역할을 한다. 
<br> .map()은 입력한 원소를 그대로 스트림으로 반환하지만, .flatMap()은 입력한 원소를 가장 작은 단위의 단일 스트림으로 반환시켜준다. 
<br> 즉, 1:N 매핑되며, 중첩된 컬렉션을 평탄화 시킬경우나, 각 요소가 리스트, 배열, 스트림인 경우 병합할 때 사용된다. 

```java
//기본적인 flatMap() 사용
List<List<String>> nestedList = Arrays.asList(Arrays.asList("a", "b"), Arrays.asList("c","d"));
List<String> flatList = nestedList.stream()
						.flatMap(Collection::stream)
						.collect(Collectors.toList());
						
System.out.println(flatList); // 출력 결과 -> [a,b,c,d]
```

### .map() 과 .flatMap() 의 차이점 
| **특징**      | **map**                        | **flatMap**                     |
| ---           | ---                             | ---                              |
| **반환 결과**  | 1:1 매핑 (변환된 개별 요소 반환) | 1:N 매핑 (하위 스트림을 병합하여 평탄화) |
| **주로 사용 시기** | 단일 변환 작업                   | 중첩 구조를 평탄화하거나 스트림 병합 시  |
| **반환 타입**  | `Stream<R>`                     | `Stream<R>` (평탄화된 단일 스트림)  |

사용 사례
<br> **map()의 사용이 적절한 경우**
1. 단순 1:1 데이터 변환: 주어진 입력 데이터를 기반으로 새로운 데이터를 생성할 때 사용합니다.
   <br> 예를 들어, 문자열을 대문자로 변환하거나 날짜를 다른 형식으로 변환하는 작업에 적합함.

2.객체의 특정 필드 추출: 객체의 특정 필드를 선택하여 새로운 스트림을 생성할 때 사용된다.
  <br> 예를 들어, 객체 리스트에서 특정 속성을 뽑아내는 작업에 유용함.

3. 값 변환 작업: 수학적 연산이나 문자열 변환 등 단순한 변환 작업이 필요할 때 사용된다.

<br> **flatMap()의 사용이 적절한 경우**
1. 중첩된 컬렉션 처리: 리스트 안에 다른 리스트가 있는 경우, 이를 단일 스트림으로 평탄화할 때 사용한다.
  <br> 예를 들어 2차원 리스트를 1차원 리스트로 변환하는 작업에 유용함.

3. 복잡한 데이터 구조 평면화: 여러 단계의 중첩 구조를 단일 스트림으로 변환하여 더 쉽게 작업할 수 있도록 한다.

4. 1:N 관계의 데이터 처리: 각 요소가 여러 하위 요소를 가질 때, 이를 병합하여 하나의 평탄화된 스트림으로 만드는 데 사용된다.
   <br> 예를 들어 사용자 정보와 해당 사용자의 모든 주문 목록이 있을 때 이를 하나의 스트림으로 합치는 경우에 적합하다.



### 실제 사용 사례 예제 코드 
#### .map()
```java
package Map;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class MapExample {
    public static void main(String[] args) {
        List<String> students = new ArrayList<>();
        students.add("Lake");
        students.add("Gary");
        students.add("Monica");
        students.add("Vicky");

        System.out.println("Students : " + students);


        //.map() 사용 -> 이름을 모두 대문자로 변환하기
        List<String> upperCaseStudents = students.stream()
                                        .map(String::toUpperCase)
                                    .collect(Collectors.toList());

        System.out.println("대문자 변환 리스트 : " + upperCaseStudents);
        //출력결과 : [LAKE, GARY, MONICA, VICKY]
    }
}
```
#### .flatMap()
```java
package Map;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class FlatMapExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Lake", Arrays.asList("Java", "Spring", "Docker")),
                new Employee("Vicky", Arrays.asList("JavaScript", "React", "Node.js")),
                new Employee("Tom", Arrays.asList("Python", "Django", "Docker"))
        );


        // 각 직원이 보유하고 있는 스킬 리스트 출력
        employees.forEach(employee ->
                System.out.println(employee.getName() + ": " + employee.getSkills()));

        // 모든 직원의 스킬 리스트를 하나의 리스트로 합치고 중복 제거
        List<String> uniqueSkills = employees.stream()
                .flatMap(employee -> employee.getSkills().stream())
                .distinct()
                .collect(Collectors.toList());

        System.out.println(uniqueSkills);
        // [Java, Spring, Docker, JavaScript, React, Node.js, Python, Django]
    }
}

class Employee {
    private String name;
    private List<String> skills;

    public Employee(String name, List<String> skills) {
        this.name = name;
        this.skills = skills;
    }
    public String getName() {
        return name;
    }

    public List<String> getSkills() {
        return skills;
    }
}

```

