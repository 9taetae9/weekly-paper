[Week 3 Topics]
###### 🇶 HashSet의 내부 동작 방식과 중복 제거 메커니즘을 설명하고, HashSet이 효율적인 중복 체크를 할 수 있는 이유를 설명해주세요.
###### 🇶 O(n)과 O(log n)의 성능 차이를 실생활 예시를 들어 설명하고, 데이터의 크기가 1백만 개일 때 각각 대략 몇 번의 연산이 필요한지 비교해주세요.
---
**🌟 "HashSet은 해시테블을 기반으로 동작하며, 내부적으로 HashMap을 사용하여 데이터를 관리한다"🌟**
먼저, Set은 중복원소를 허용하지 않는 데이터 자료구조이다. Hash는 임의의 길이를 갖는 데이터를 고정된 길이의 데이터로 변환(매핑)하는것을 말한다. 

### HashSet의 동작원리
ArrayList, LinkedList와 같은 자료구조의 경우 특정 값을 찾으려 할때 배열 또는 노드를 다 순회하면서 특정값을 찾는 방식으로 동작한다. 
<br> 하지만 HashSet에서 해시함수를 사용하여 동작하면 굳이 순회할 필요가 없다. 
- 동일한 값에 대해 동일한 다이제스트(해시함수에서 얻은 값)를 얻게 되기 때문이다. 이 특정 값에 대한 다이제스트는 변하지 않기 때문에 이 값을 배열의 위치(인덱스)로 활용하게 된다. 

또한, ArrayList, LinkedList와 같은 자료구조의 경우 원소를 add 할때마다 해당 원소가 중복인지 아닌지 검사해야 하는식으로 동작한다. 이때, 모든 원소를 검사하면서 진행한다면 매우 비효율적이다. 
- 이때, hash함수를 통해 특정값의 고유한 키 값 다이제스트를 얻고 그 값에 대응하는 인덱스를 찾아 해당 인덱스에 있는 요소만을 검사하면 된다.

즉, HashSet은 내부적으로 HashMap을 사용하여 데이터를 관리한다. 
HashSet에 값을 저장할 때 그 값을 HashMap의 키로 저장되는것이다. 

📍 HashSet의 add() 메소드는 값을 추가할 때 내부적으로 두가지 과정을 거친다. 
1. hashcode() : 객체의 해시코드를 생성해준다. 객체를 좀 더 효율적으로 비교하기 위해 사용한다. 
<br>즉, hashCode()를 호출하여 객체의 해시값을 계산한다. > 사용하지 않으면 객체가 주소를 가르키게 되기 때문에 꼭 오버라이딩 해줘야 한다.
2. equals() : 객체 간의 동등성을 비교한다. 즉, 이 메소드를 호출하여 해당 해시값의 슬롯에 이미 동일한 객체가 있는지를 확인하게 된다.

### HashSet의 중복제거 메커니즘
- HashSet은 각 객체의 해시코드를 사용해서 저장 위치를 결정하고, 동일한 해시코드가 두번 나오지 않도록 관리한다.
- equals() 메소드 사용으로 두 객체가 동일한 값을 가지는지 확인한다. 동일한 해시 코드의 객체가 추가될 때마다 equals() 메소드를 호출하여
  중복된 요소를 확인하고, 동일한 객체가 있는 경우 추가를 거부하도록 동작한다.

### HashSet이 효율적인 중복체크를 할 수 있는 이유
 - 내부적으로 해시 테이블을 사용하기 때문에 평균적으로 O(1) 의 시간 복잡도로 요소를 삽입, 검색, 삭제할 수 있기 때문에, 중복확인에서 효율적이다.
 - 해시테이블 충돌 처리로 중복된 객체 효과적으로 관리가 가능하다.
   - 해시충돌 방지 2가지 방법
     1. Open Addressing 
        [장점]
        - 해시충돌이 일어날 가능성이 적기 때문에 데이터의 개수가 적을수록 속도가 빠르다
        - 연속된 공간에 데이터를 저장하기 때문에 캐시 효율이 좋아진다.
        [단점]
        - 해시충돌이 발생하면 해당 객체의 인덱스값이 바뀌어 버린다.
        - 부하율(load factor), 즉, 테이블에 저장된 데이터의 개수가 많아질수록(밀도가 높아질수록) 성능이 급격히 저하된다. 
     2. Seperating Chanining
       [장점]
        - 해시충돌이 일어나더라도 연결리스트로 노드가 연결되기에 index가 변하지 않는다.
        - 테이블의 각 인덱스는 연결리스트로 구현되기 때문에 데이터 개수의 제약을 거의 받지 않는다.
        [단점]
        - 부하율에 따라 선형적으로 성능이 저하될수 있기에 부하율이 작다면 Open Addressing 방식이 더 빠를수 있다.
          
![다운로드 (2)](https://github.com/user-attachments/assets/461c4141-9a6f-40d8-aa49-f9718f24a12b)
- hashcode()와 equals() 메소드를 통해 객체의 고유성을 확인하고, 중복된 객체를 제거하기 때문에 메모리 사용량과 성능이 최적화된다.       
---
Big O 표기법
- 알고리즘 수행에 필요한 단계 수를 통해 알고리즘의 효율성을 쉽게 분류할 수 있다.
- 일관되고 간결한 방법으로 어떤 알고리즘이든 분석할 수 있는 도구

  
### O(n)과 O(logn)
O(n)과 O(logn)은 알고리즘의 시간 복잡도를 나타내며, 각각의 수행시간이 데이터의 크기와 어떤 관계가 있는지를 정의한다.
#### O(n) 과 O(log n)의 개념
1. O(n) : 데이터 크기가 증가함에 따라 수행시간이 비례해서 증가하는 경우 (=데이터 원소가 N개라면 N단계가 걸린다)
<br> 모든 요소를 단순히 순차적으로 탐색하는 경우 -> 선형시간

즉, 입력값이 증가함에 따라 같은 비율로 증가하는것이며, 주로 반복문을 통해 값을 찾을 때 이 시간복잡도를 가지게 사용하게 된다.

```
public class TimeComplexity {
    public static void main(String[] args) {

        int[] ints = {1,2,3,4};


        O1 o1 = new O1();
        int num = o1.play(ints,2);
        System.out.println(num);        //출력 1 (숫자 2의 index값)

    }
}

class O1{
    int num;
    int play(int[] nums,int n){

        for(int i=0; i<nums.length; i++){
            if(nums[i]==n){
                num=i;
            }
        }
        return num;
    }
}

/*간단한 반복문을 통해 n이 가지는 index 값을 찾아내는 코드*/
```

2. O(logn) : 데이터 크기가 증가함에 따라 수행시간이 로그 형태로 증가하는 경우
<br> 이진 검색이나 균형잡힌 이진 트리 탐색하는 경우 -> 데이터 크기의 로그 값만큼의 연산만 필요하다. -> 로그시간

즉, 단순 순환이 아닌 1/2씩 경우의 수를 줄여가며 탐색하는 알고리즘을 이용할 때 사용하는 표기법이다.

```
class Tree {
  int count = 0;
  class Node {
    int data;
    Node left;
    Node right;
    Node(int data) {
      this.data = data;
    }
  }

  Node root;
  public void makeTree(int[] a) {
    root = makeTreeR(a, 0, a.length -1);
  }
  public Node makeTreeR(int[] a, int start, int end) {
    if (start > end) return null;
    int mid = (start + end) /2
    Node node = new Node(a[mid]);
    node.left = makeTreeR(a, start, mid -1);
    node.right = makeTreeR(a, mid+1, end);
    return node;
  }
  public void searchBTree(Node n, int find) {
    if(find < n.data) {
      System.out.println("Data is smaller than" + n.data);
      searchBTree(n.left, find);
    } else if (find > n.data) {
      System.out.println("Data is bigger than" + n.data);
      searchBTree(n.right, find);
    } else {
      System.out.println("Data found");
    }
    count++;
    System.out.println("반복횟수는" + count + "입니다");
  }
}

//Test
public class Test {
  public static void main(String[] args) {
    int[] a = {1,2,3,4,5,6,7,8,9,10};
    Tree t = new Tree();
    t.makeTree(a);
    t.searchBTree(t.root, 1);
  }
}

//출력결과 
//Data is smaller than 5
//Data is smaller than 2
//Data found!
//반복횟수는 1 입니다
//반복횟수는 2 입니다
//반복횟수는 3 입니다

/* 즉, 전체 배열의 가운데에 위치한 값을 기준으로 왼쪽에 본인보자 작은 수, 오른쪽에 본인보다 큰 수로 나누어 
이걸 반복해 Tree를 만들고 그 트리를 순환해서 값을 찾아낸다. -> 이 과정의 경우의 수를 반으로 계속 줄이며 탐색하는 과정을 반복한다.*/
```
![효율성](https://github.com/user-attachments/assets/add41e28-5029-4e4c-8292-88e284f7c9aa)

### O(n) 과 O(logN)의 실생활 예시 성능비교
전화번호부에 있는 모든 사람의 이름과 번호를 일일이 확인해야 하는 경우 
<br> O(n) 이라면 연락처에 100명이 있다고 할때 원하는 이름을 검색하고 싶다면 각 이름을 모두 확인해야 하기 때문에 약 100번의 연산이 필요하다.
 <br> 이후, 갯수가 1000, 10000명으로 늘어나면 똑같이 연산수도 1000, 10000번으로 늘어난다. 
<br> 반면 O(logN) 이라면 이 정렬되어있는 데이터에서 특정 이름을 검색하기 위해 이진 검색을 사용하기에 연산수를 절반으로 줄여나가며 더 빠르게 검색할 수 있다. 


### 데이터 크기가 100만개일때 
- O(n)의 경우, 데이터 크기 n 이 100_000이라면 연산 횟수도 100만번이 필요하다. 
<br> 즉, 연산 수는 O(n) = O(1000000) = 1000000번의 연산이 필요하다.
- O(logN) 의 경우 100만개의 데이터를 탐색할때 로그의 형태로 증가하게된다. 
<br> 즉, 연산 수는 O(log 1000000) = O(20) 정도이며, 약 20번의 연산이 필요하다.
