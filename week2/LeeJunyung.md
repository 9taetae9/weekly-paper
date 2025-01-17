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

