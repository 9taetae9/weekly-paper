[1. 단일 책임 원칙과 개방 폐쇄 원칙](#단일-책임-원칙과-개방-폐쇄-원칙)

[2. 스트림 api](#stream-api)

# 단일 책임 원칙과 개방 폐쇄 원칙
# 🔎 주제
> 객체지향 프로그래밍에서 '단일 책임 원칙(SRP)'과 '개방-폐쇄 원칙(OCP)'에 대해 설명하고, 각각의 원칙을 적용한 코드 예시를 들어주세요.

객체 지향 프로그래밍에는 **'SOLID'** 원칙이 있다.
Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation, Dependency Inversion 이 5가지 원칙을 말하는데 오늘은 그 중 두 개의 원칙에 대해 다룬다.


# ✅ 단일 책임 원칙(SRP)
**단일 책임 원칙Single Responsibility, SRP**는 **클래스는 하나의 책임만 가져야 한다**는 원칙이다.

- 책임: 클래스가 처리해야 하는 하나의 변경 이유
쉽게는 클래스가 담당해서 처리해야 하는 하나의 "일"이라고 볼 수 있다. 클래스는 자신이 맡은 특정한 "일"만 처리해야 한다. 하나의 클래스는 단 한가지의 이유로만 변경되어야 한다.

하나의 클래스가 맡는 책임이 많아질수록, 클래스는 복잡해지고 수정과 유지보수가 어려워진다. 한 일을 변경했다가 다른 일에 영향을 끼칠 수 있기 때문이다.

## ☕ 예시 
일상생활을 예시로 생각해보자.

> 카페에서 직원이 한 명만 있다.
이 직원은 커피를 제조하고, 계산도 하고, 청소도 한다.

한 명의 직원이 여러 업무를 맡고 있다.
만약 커피 제조 레시피가 바뀌고, 포스기 시스템도 바뀌고, 청소 구역도 바뀐다면 적응하기 쉽지 않을 것이다.

- ```직원=클래스```, ```업무=책임```, ```레시피, 시스템```, ```구역 바뀜=변경 이유```, ```적응=클래스 수정/변경```이라고 생각해보자.

> 카페에 직원이 늘었다.
바리스타는 커피를 만들고, 캐셔는 계산을 하고, 청소부는 청소를 한다.

커피 메뉴가 바뀌어도 캐셔와 청소부는 신경쓰지 않는다.
포스기 시스템이 바뀌어도 바리스타와 청소부는 신경쓰지 않는다.
자신의 책임이 아니기 때문이다.

업무 분장(=책임을 나누는 것)이 효율적이고 안정적인 운영을 위한 해답이다.

## 🖥️ 예시 코드

- 단일 책임 원칙을 지키지 않은 경우
  
커피 제조와 주문 받기, 두 가지 일을 하고 있다.

이 클래스는 음료 레시피가 바뀌거나, 또는 현금만 받는 등 결제 시스템이 바뀌는 등 두 가지의 이유로 변경될 수 있다.
```
public class Cafe {

	// 커피 제조
	public Coffee makeCoffee(String menu) {
    	if(메뉴=="아메리카노") {
        	원두 추출;
            물 따르기;
        } else if(메뉴=="카페라떼") {
        	원두 추출;
            우유 스팀하기;
        }
        ...
    }
    
    // 주문 받기
    public void createOrder(String menu) {
    	포스기에서 메뉴 선택;
     	...
    }
    
}
```

- 단일 책임 원칙을 지킨 경우
  
바리스타는 커피 제조를, 캐셔는 결제를 처리한다.
```
public class Barista {
	// 커피 제조
	public Coffee makeCoffee(String menu) {
    	if(메뉴=="아메리카노") {
        	원두 추출;
            물 따르기;
        } else if(메뉴=="카페라떼") {
        	원두 추출;
            우유 스팀하기;
        }
        ...
    }
}

public class Casher {
	// 주문 받기
    public void createOrder(String menu) {
    	포스기에서 메뉴 선택;
     	...
    }
}
```

각각의 클래스가 명확하게 자신만의 책임을 가지므로 테스트, 수정, 유지보수가 쉬우며 독립적이므로 재사용도 쉽다.

# ✅ 개방-폐쇄 원칙(OCP)
**개방-폐쇄 원칙Open-Closed**은 **확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다**는 원칙이다.

> 아니.. 단일 책임 원칙에서는 수정하기 쉬워진다면서요? 변경에는 닫혀 있다니, 수정하지 말라는 건가요?

이 원칙은 모든 변경에 대해 말하는 게 아니다. 확장될 때의 변경을 말한다.

**기존의 코드를 변경하지 않으면서도 시스템의 기능을 확장할 수 있어야 한다.**
개방-폐쇄 원칙에 따르면, 새로운 기능을 추가할 때 기존의 코드에 영향을 주지 않고도 추가할 수 있어야 한다.

요지는, 이미 잘 돌아가던 기존 코드를 확장한다고 건들이지 말고 새로운 요구사항만 구현하라는 뜻이다.

이때 이용하기 좋은 것이 추상화와 인터페이스다. 예시 코드로 보자.

## 🪙 예시 코드
- 개방 폐쇄 원칙을 지키지 않은 경우
결제 시 고객이 지불해야 하는 가격을 알려야 한다.
```
public class Casher {
	// 가격 찾기
    public int getPrice(String menu) {
    	if(메뉴==아메리카노)
        	return 2000;
        else if(메뉴==카페라떼)
        	return 3000;
 		else if ...
    }
}
```
만약 여기에서 신메뉴 버블티가 추가된다면? else if()을 추가해야 할 것이다. 신메뉴로 확장되지만 기존 코드가 변경되는 것이다.

- 개방 폐쇄 원칙을 지킨 경우
메뉴가 몇 개가 추가되든 기존의 코드들은 변하지 않는다.
```
interface Drink {
	int getPrice();
}

class Americano {
	public int getPrice() { return 2000; }
}

class CafeLatte {
	public int getPrice() { return 3000; }
}

class BubbleTea {
	public int getPrice() { return 5000; }
}
```

개방-폐쇄 원칙은 유연하게 확장이 가능하면서도 기존 코드를 건드리지 않으므로 안정적이다.

# 🔑 요약

- 단일 책임 원칙
	- "클래스는 하나의 책임만 가져야 한다."
    - 하나의 클래스는 단 한가지의 이유로만 변경되어야 한다.
    - ✅ 유지보수 good: 코드 변경이 필요한 이유가 분리되고 명확하여 수정이 쉽다.
    - ✅ 재사용성 good: 각 클래스가 독립적이므로 나중에 해당 기능이 필요한 상황에서 쉽게 재사용이 가능하다.
    - ✅ 테스트 good: 한 가지 일을 잘하는지 테스트하면 되니 쉽다.
    
- 개방-폐쇄 원칙
	- "확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다."
    - ✅ 안정성 good: 기존 코드를 건드리지 않으니, 이미 잘 돌아가던 기능은 에러가 나지 않는다.
    - ✅ 유연성, 확장성 good: 새로운 기능을 쉽게 추가하기 편하다.
    - **추상화와 인터페이스**를 잘 활용하자!

# 블로그
[링크](https://velog.io/@soo7132/%EC%9C%84%ED%81%B4%EB%A6%AC-%ED%8E%98%EC%9D%B4%ED%8D%BC-2-1.-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-SOLID-%EC%9B%90%EC%B9%99-%EC%A4%91-%EB%8B%A8%EC%9D%BC-%EC%B1%85%EC%9E%84-%EC%9B%90%EC%B9%99SRP%EA%B3%BC-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99OCP)


---

# stream api

# 🔎 주제
> Stream API의 map과 flatMap의 차이점을 설명하고, 각각의 활용 사례를 예시 코드와 함께 설명해주세요.

주제에 언급된 map(), flatMap() 외에 다른 메소드와 개념도 참고용으로 정리해봤습니다~

# ✅ Stream

## Collection Framework
- 데이터를 저장하고 조작하는 구조와 인터페이스를 제공하는 자바 라이브러리
- List, Set, Map들이 포함되고 데이터를 저장, 검색, 삭제하는 등의 작업을 처리한다.
- 위 구조들에 대해서는 차후 다른 글로 다루겠다.


## Stream
- 컬렉션 데이터를 처리하기 위해 데이터 소스와 연결되어 영속된 데이터 흐름을 제공한다. 
    - 데이터를 일회성으로 처리하기 위한 도구.
    - 영속: "스트림 작업 후에도 데이터가 변화하지 않고 지속적으로 존재한다"는 뜻이다. 
    - 불변성: 처리된 결과도 새로운 데이터로 생성하므로 원본 데이터는 변경되지 않는다.

- 지연 처리: 중간 연산과 최종 연산
    - 중간 연산: 최종 연산이 호출될 때까지 실행되지 않고, 대기하고 있다가 최종 연산이 호출되면 한 번에 동작한다. ex) map(), filter(), sorted()...
    - 최종 연산: 스트림을 소모해서 결과를 생성한다. 최종 연산이 호출되면 이 스트림은 닫혀 사용할 수 없다. -> 데이터 불변성을 보장.
    - 불필요한 연산을 줄여 효율적으로 동작한다.
 
- 함수형 프로그래밍 스타일을 사용한다.(feat. 람다 표현식)

### ✅ Stream의 장점

- 가독성이 높다: 함수형 프로그래밍 스타일은 간결하고, 익숙해지면 읽기도 쉽다.
- 쉬운 유지보수: 간결한 데이터 처리 로직이라 수정이 쉽다.
- 병렬 처리를 통한 성능 향상: ```parallel()``` 또는 ```parallelStream()```을 사용하면 데이터 처리를 병렬화할 수 있다.

### ❎ Stream의 단점

- 경우에 따라 오버헤드가 발생할 수 있다. 작은 데이터셋에는 단순한 반복문이 낫다.
- 디버깅이 어렵다.

# ✅ 주요 Stream API
Stream API는 스트림에 있는 연산 기능을 말한다. 중간 연산과 최종 연산으로 나누어 진다.

- 중간 연산: 데이터를 변화나거나 필터링하는 등 하나의 스트림에서 여러 연산을 연속해서 사용 가능하다.
    - map(), filter(), sorted(), ...
- 최종 연산: 스트림을 소모해서 결과를 생성하므로 하나만 가능하다.
    - forEach(), collect(), reduce(), ...

## 🛠️ 중간 연산
### filter()
조건에 맞는 요소만 선택한다.
```
List<Integer> numbers = Arrays.asList(1,2,3,4,5);
numbers.stream()
	.filter(n -> n >2 )
    .forEach(System.out::println);  
--------------------------
 3
 4
 5
```
### map()
데이터를 변환해서 새로운 형태로 변환한다. 예시에서는 모두 대문자로 바꾼다.
```
List<String> names = Arrays.asList("Alice", "Bob", "Christine");
names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);     
--------------------------
ALICE
BOB
CHRISTINE
```
### flatMap()
중첩된 데이터 구조를 단일 평면 데이터 스트림으로 변환한다. 즉 여러 계층으로 중첩된 데이터를 풀어 플랫하게 만든다.
```
List<List<String>> nestedList = Arrays.asList(
            Arrays.asList("A", "B"),
            Arrays.asList("C", "D"),
            Arrays.asList("E", "F")
        );

        List<String> flatList = nestedList.stream()
        	.flatMap(List::stream)
            .collect(Collectors.toList());
System.out.println(flatList);
--------------------------
[A, B, C, D, E, F]
```

map()과 다른 점은 "입력 개수와 출력 개수가 반드시 같지는 않다"는 점이다.

map()의 예시에서 입력 데이터와 출력 데이터에서 다음 매핑 관계가 성립한다.
- ```Alice -> ALICE```
- ```Bob -> BOB```
- ```Christine -> CHRISTINE```

하나의 입력에 대해서 하나의 출력 결과가 나오는 1:1 관계이다.

flatMap()에서는 다음 매핑 관계가 성립한다.
- ```("A", "B") -> "A", "B"```
- ```("C", "D") -> "C", "D"```
- ```("E", "F") -> "E", "F"```

nestedList는 중첩 리스트로 각각의 요소에 접근해도 여러 값으로 구성된 리스트이다.
nestedList에서의 stream()은 리스트 ("A" ,"B")라는 하나의 입력을 받아 "A", "B"로 나눈다. 

즉 중첩 구조였던 nestedList를 일차원으로 폈다고 볼 수 있다.
collect()를 이용한다면 다음처럼 플랫해진 리스트를 따로 얻을 수 있다.
```
List<String> flatList = nestedList.stream()
                                .flatMap(List::stream)
                                .collect(Collectors.toList());
```

### sorted()
기준에 따라 정렬한다. 기준을 따로 주지 않아도 알아서 오름차순으로 정렬한다.
```
numbers.stream()
	.sorted()
    .forEach(System.out::println);
```

원하는 기준을 넣고 싶다면 ```Comparator```나 ```Comparator.comparing()```을 이용하자.

- ```Comparator```
```
List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 3);
List<Integer> sorted = numbers.stream()
									.sorted((a, b) -> b - a)
                                    .collect(Collectors.toList());
--------------------------
[8, 5, 3, 2, 1]
```
- ```Comparator.comparing()```
```
List<String> words = Arrays.asList("appleJuice", "banana", "apricot!", "cherry");

List<String> sorted = words.stream()
        	.sorted(Comparator.comparing(String::length)) //길이 기준 오름차순
            .collect(Collectors.toList());
--------------------------
[appleJuice, apricot!, banana, cherry]
```

### distinct()
중복 요소를 제거한다.
```
List<String> names = Arrays.asList("Alice", "Alice", "Bob", "Cindy");
names.stream()
	.distinct()
    .forEach(System.out::println);
--------------------------
[Alice, Bob, Cindy]
```

### skip()
처음 n개의 요소를 건너뛰고 연산한다.
```
List<Integer> numbers = Arrays.asList(1,2,3,4,5);
numbers.stream()
	.skip(2)
    .forEach(System.out::println);  
--------------------------
 3, 4, 5
```

### limit()
스트림이 가져오는 요소 개수를 n개로 제한한다.
```
List<Integer> numbers = Arrays.asList(1,2,3,4,5);
numbers.stream()
	.limit(3)
    .forEach(System.out::println);  
--------------------------
1, 2, 3
```


---

## 🛠️ 최종 연산
### forEach()
각 요소를 소비한다.
```
numbers.stream()
	.forEach(System.out::println);	// 메소드 레퍼런스 방식

numbers.stream()
	.forEach(num -> {
    	System.out.println(num);	// 람다 방식
    });	// 각 요소 출력
```
### collect()
결과를 특정 컬렉션으로 반환한다.
```
List<Integer> numbers = Arrays.asList(1, 2, 3);
// 각 요소를 제곱한 리스트로 반환
List<Integer> squares = numbers.stream()
		.map(number -> number*number)
    	.collect(Collectors.toList());

// 단어를 키로, 길이를 값으로 하는 맵으로 반환
List<String> words = Arrays.asList("apple", "banana", "cherry");
Map<String, Integer> result = words.stream()
		.collect(Collectors.toMap(word -> word, String::length));
```

### reduce()
모든 요소를 결합해서 하나의 결과를 생성한다.
```reduce(초기값, 결합방식)``` 방식으로 사용한다.

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

//각 요소의 값 더하기
int sum = numbers.stream()
	.reduce(0, (a,b) -> a + b);	//초기값 0에서부터 두 값을 더함
    
//최댓값 구하기
int max = numbers.stream()
	.reduce(Integer.MIN_VALUE, (a, b) -> a > b ? a : b); // Integer.MIN_VALUE로 초기화
```

### anyMatch(), allMatch(), noneMatch()
일치하는 요소가 있는지 찾는다. 결과는 boolean으로 반환한다.
- anyMatch(): 일치하는 요소가 하나라도 있으면 true
- allMatch(): 모든 요소가 조건을 만족하면 true
- noneMatch(): 모든 요소가 조건을 만족 못하면 true
```
List<String> names = Arrays.asList("Alice", "Bob", "Cindy", "Daniel", "Edward", "Felilx");

// false
boolean hasLongName = names.stream()
	.anyMatch(name -> name.length() > 10);

//true
boolean allShortNames = names.stream()
	.allMatch(name -> name.length() < 10);

//true
boolean noNameStartWithZ = names.stream()
	.noneMatch(name -> name.startsWith("z")); // "z" 로 시작하는 이름이 없는지 확인
```

### findFirst(), findAny()
조건을 만족하는 요소를 반환한다.
- findFirst(): 조건을 만족하는 요소 중 첫 번째를 반환한다.
- findAny(): 조건을 만족하는 요소 중 하나를 반환한다. 순차 스트림에서는 첫 번째 요소를, 병렬 스트림에서는 어떤 요소든지 반환될 수 있다.

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 4 반환
Optional<Integer> first = numbers.stream()
	.filter(n -> n > 3) // 3보다 큰 수만 필터링
    .findFirst(); // 첫 번째 요소 반환
```

### count()
스트림에서 요소의 개수를 반환한다. filter()와 함께 사용해도 좋다.
```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

int count = numbers.stream()
						.count(); // 스트림의 요소 개수 세기

int even = numbers.stream()
						.filter(n -> n % 2 == 0) // 짝수만 필터링
                        .count(); // 필터링된 요소의 개수

```

## Map에서 Stream API를 쓰는 법

Stream API는 컬렉션 데이터를 처리하기 위한 메소드라고 했다.
이 컬렉션 데이터란, Collection interface를 구현하는 구현체 데이터를 말한다.
여기엔 List, Set 등이 포함된다.

다만 List와 Set만큼 자주 쓰는 Map은 포함되지 않는다!
Collection Interface 중 Set을 활용한다.

Map에서 나올 만한 Set은 key set, value set, entry set이다. 모두 Map에 내장된 메소드를 이용해 구할 수 있다. 이 Set 형태의 데이터들로 Stream API를 쓰면 된다.

- keySet()
- values()
- entrySet()

# 블로그
[[링크 🔗](https://velog.io/@soo7132/%EC%9C%84%ED%81%B4%EB%A6%AC-%ED%8E%98%EC%9D%B4%ED%8D%BCJava-2-2.-Stream-%EA%B0%9C%EB%85%90%EA%B3%BC-Stream-API-%EC%B4%9D%EC%A0%95%EB%A6%AC)]
