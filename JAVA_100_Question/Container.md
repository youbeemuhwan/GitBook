# Container

### <mark style="color:yellow;">자바에서 컨테이너 란?</mark>

컴포넌트를 담는 그릇 이다. 컴포넌트란 JSP, 서블릿 등 웹 컴포넌트도 포함한다.

해당 컴포넌트의 생명주기,  생성 및 초기화, 종료 등의 컴포넌트 관리 역할을 한다.



또한

Java.util 라이브러리에는 컨테이너 클래스가 있다.

Collection(List, Set, Queue), Map (LinkedHashMap, HashMap) 이 여기에 속한다.

### <mark style="color:yellow;">Collection과 Collections의 차이는 무엇인가요?</mark>

Collection 은 자바에서 모든 컬렉션 클래스 , 인터페이스를 포함하는 프레임워크를 말한다.

Collections 는 Collection 들 에 대한 연산을 수행하거나 Collection 객체를 반환하는 Static 함수들의 모음이다.

### <mark style="color:yellow;">List, Set, Map의 차이점을 말해주세요.</mark>

List

중복이 가능하며 순서를 보장하는 컬렉션이다.

Set

순서를 보장하지 않고 중복이 불가능한 컬렉션이다.

Map

("key" :"value") 값을 가지는 컬렉션이다. key는 중복이 불가하며 value는 중복이 가능하다.

## <mark style="color:yellow;">HashMap과 Hashtable의 차이는 무엇인가요?</mark>

Hashtable 이란 (key, value) 로 데이터를 저장하는 자료구조를 뜻한다.

HashMap 은 Map 형태를 지니면서 해싱을 통해서 많은 양의 데이터를 검색하는데 있어서 뛰어난 성능을 보이는 자료형이다.

#### 차이점

1. **동기화 여부**

병렬 상태의 환경에서 사용해야한다면 동기화를 지원해주는 HashTable이 유리하고

멀티쓰레드 환경이 아니라면 동기화 지원이 안되는 HashMap 이 유리하다.

2. **Null 값 허용 여부**

HashMap : Null 허용

HashTable : Null 허용 X

## <mark style="color:yellow;">각각 어떤 상황에서 HashMap과 TreeMap을 선택하나요?</mark>

TreeMap : 이진트리를 기반으로 한 Map 컬렉션이다. 중복은 허용되지 않고 sortedMap 을 사용해 일정한 키값을 가지며 정렬상태를 유지한다는 특징이 있다. 하지만 정렬을 하기 때문에 저장시간이 길고 트리 구조 특성상 특정엔트리 접근에 시간이 HashMap 보단 좀 더 걸린다.

HashMap : 중복을 허용하지 않으며 해싱값을 키값으로 사용해 순서가 일정하지 않다는 특징이 있다.

해싱을 사용하기에 검색속도가 굉장히 빠르다는 장점이 있다.

일정한 키값으로 정렬상태를 유지해야 한다면 TreeMap

위와 같은 이유가 없다면 무조건 HashMap 을 선택하는것이 성능면에서 장점을 보인다.

## <mark style="color:yellow;">HashMap 구현 원칙은 무엇인가?</mark>

HashMap 은 hashCode() % BUCKET\_SIZE 공식을 사용해 데이터를 어떤 버킷(슬롯)에다 저장, 조회 할것인지 판단합니다.

그리고 HashMap 구조 특성 상 해시충돌이 발생할수밖에 없는데 이는 개방주소법, 체이닝 기법을 사용해서 완화한다.

* **개방주소법**

충돌이 일어났을때 다른 버킷 위치를 탐색해서 비어있는 곳으로 옮기는 방법이다.

* **Seperate Chaining**

데이터를 버킷(슬롯) 에 저장 할 때 그냥 넣는것이 아닌 Linked List 형식 처럼 넣는 방식이다.

실제로 Linked List 는 아니지만 데이터를 그대로 넣는것이 아니라는 점을 잘 기억하자

### 해시 버킷 크기의 동적 확장

버킷 크기가 작다면 메모리 사용을 아끼겠지만 너무 적다면 충돌 시 성능 손실을 야기한다.

해시는 버킷 크기의 기본값은 16개 이다. 하지만 항상 데이터가 16개 이하 일수는 없다.

만약 데이터가 특정 임계점을 넘어가게되면 버킷의 크기를 2배로 늘리도록 동적 확장된다.

임계점의 기준은 LoadFactor \* 버킷 개수 를 기준으로 한다.

LoadFactor 는 개발자가 설정이 가능하며 기본값은 0.75 이다.

하지만 이렇게 동적확장이 이루어질때 마다 체이닝을 전체적으로 돌면서 다시 조정하기에 자주 진행한다면 성능에 손실이 올수있기에 데이터가 예상 가능한 범위라면 넉넉하게 버킷 크기를 지정해서 사용하는게 가장 베스트한 방법 인거 같다.

### 보조 해시 함수를 이용한 균등 분포

이렇게 체이닝 기법으로 각각 버킷에 담겨지다 보면 어떤 한 버킷에만 데이터가 몰릴수도 있다.

버킷마다 균등하게 데이터를 분포 해줘야 한다는 것이다.

자바에서는 보조 해시 함수를 이용하고있다.

버킷의 수가 보통 소수일때 해시 충돌이 적게 일어나고 2^n 이라면 해시 충돌이 확률이 높아진다고 한다.

그렇기에 보조해시 함수를 통한 연산으로 key 해시값 을 조정해서 균등분포에 도움을 준다.

### <mark style="color:yellow;">HashSet 구현 원칙은 무엇인가요?</mark>

해싱을 사용해서 Set 컬렉션을 사용하는 컬렉션이다.

기존 Set 와 마찬가지로 중복을 허용하지 않는다.

값을 저장하기 전에 해시 값을 비교해서 해당 해시 값이 이미 있는 해시 값이면 중복처리 한다.

또한 String 값을 넣게되면 equals() 를 오버라이딩 해서 문자열값만 같아도 true 값이 나오도록 되있다.

### <mark style="color:yellow;">ArrayList와 LinkedList의 차이점은 무엇인가요?</mark>

* **ArrayList**

장점

1. 중복을 허용한다.
2. 인덱스를 가지고 있어 조회/탐색에 있어 강력한 성능을 보여준다.
3. 만약 리스트의 길이를 초과한다면 자동으로 크기를 늘려준다.

단점

1. 데이터 삽입, 수정 시 에 기존 데이터들을 앞 뒤로 옮겨야 하기 때문에 성능 저하가 야기된다.
2. 크기를 늘려주긴 하지만 연산량이 많이 요구되어서크기가 한정적이다 라는 표현이 쓰인다.

* **LinkedList**

장점

1. 중복을 허용한다.
2. 각 노드가 데이터, 포인터를 가지고 일렬로 연결되있는 형태이다. 인덱스 X
3. 각 노드 별로 되있기에 자료 수정, 삭제 시 리스트 내 데이터 이동이 없다.
4. 자료 수정, 삽입이 용이하다.

단점

1. ArrayList 와 달리 불규칙적으로 나열되있기에 탐색 시 시간이 많이 소요된다.
2. 포인터 사용으로 인한 저장공간 낭비가 있다.

### <mark style="color:yellow;">Array에서 List로 전환하려면 어떻게 해야하나요?</mark>

1. for문 사용

```
  String[] strings = {"a", "b", "c"};

        List<String> list = new ArrayList<>();

        for (String string : strings){
            list.add(string);
        }

        System.out.println("list = " + list);

```

2. asList 사용

```
 String[] strings = {"a", "b", "c"};
        
        List<String> list = Arrays.asList(strings);
```

우리가 평소에 사용하는 ArrayList 는 java.util 에 속해있지만 asList 에서 사용하는 ArrayList 는 java.util.Arrays 에 속해있다. 그렇기에 기존 java.util.ArrayList 와는 지원하는 메서드도 다르며

java.util.Arrays.ArrayList 는 안에는 배열이 담겨있기에 기존 배열이 변경되면 해당 리스트 값도 변경된다는 점을 주의해야한다.

3. Stream.collect() 사용

```
 String[] strings = {"a", "b", "c"};

        List<String> collect = Arrays.stream(strings).collect(Collectors.toList());
```

## <mark style="color:yellow;">ArrayList와 Vector의 차이점을 말해주세요.</mark>

Vector 는 ArrayList 와 동일한 구조를 가지고 있다.

하지만 항상 동기화 되있다는 점

Collection 프레임워크에 없는 메서드 사용이 가능하다는점

동기화 라는 특징으로 인해 스레드 환경이 아니라면 잘 사용하지 않고 삽입. 삭제. 수정 등 의 성능이

ArrayList 에 비해 떨어진다는 점을 차이점 으로 들수있다.

## <mark style="color:yellow;">Array와 ArrayList의 차이점을 말해주세요.</mark>

Array 와 ArrayList 는 구조는 거의 동일하다.

**공통점**

1. add, get 메서드 사용
2. index 사용
3. 순서 보장 X , 중복 허용

**차이점**

크기를 가변적으로 바꿀수있냐 없냐 가 가장 큰 차이이다.

Array 는 크기가 정하고 후에 늘리거나 줄일수 없다.

ArrayList 는 삽입되는 데이터의 크기에 따라 늘어나기에 가변길이를 가지고 있다.

## <mark style="color:yellow;">Queue에서, poll()과 remove()의 차이는 무엇인가요?</mark>

Queue 의 맨 앞에 있는 값을 반환후 삭제 하는 동작은 동일하지만 리턴값이 null 일 경우

poll() null 값을 반환하지만 remove() 는 NoSuchElementException 오류를 던진다는 차이점이 있다.

## <mark style="color:yellow;">thread-safe한 컬렉션 클래스들은 무엇이 있을까요?</mark>

Vector, HashTable 이 있다.

다만 동기화를 위한 Collection 에 synchronizedXXX 메서드가 있다.

synchronizedList(), synchronizedSet(), synchronizedMap() 등이 있고

synchronizedMap(Map map) 을 넣으면 동기화를 도와준다. 하지만 스레드를 처리할때 락이 발생해서

그동안은 다른 스레드를 처리할수없다는 단점이 있다.

## <mark style="color:yellow;">iterator란 무엇인가요?</mark>

Iterator 는 반복자 라는 뜻을 가지고 있다. 컬렉션에 저장되있는 요소들을 읽어오는 방법을 표준화 한것이다.

Collection을 iterator 화 해서 아래에 메서드를 사용하여 For문. while 문으로 요소들을 가져올수있다.

**hasNext()** : 읽어올 요소가 남아있는지 확인하는 메소드이다. 요소가 있으면 true, 없으면 false

**next()** : 다음 데이터를 반환한다.

**remove()** : next()로 읽어온 요소를 삭제한다.

사용이 쉽고 간단하다. 그리고 List 같은 컬렉션을 순차적으로 접근할수있다는 점에서 장점을 가지고 있다.

## <mark style="color:yellow;">iterator와 listIterator의 차이는 무엇인가요?</mark>

listierator 는 iterator 을 상속받아 기능을 추가해서 만든 인터페이스 이다.

기존 iterator 는 next 방향으로만 움직이는게 가능했다면 listierator 는 양방향이 가능하다.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>
