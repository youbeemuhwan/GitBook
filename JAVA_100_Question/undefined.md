# 기초

## 기초

* <mark style="color:yellow;">**JDK 와 JRE의 차이점은 무엇입니까?**</mark>

JRE 는 Java Runtime Environment 의 약자로 의역하면 자바 구동 환경 이다.

말 그대로 자바 프로그램을 실행하기 위한 라이브러리, 각종 API, JVM 등이 포함되어있다.

JDK 는 Java Development Kit 의 약자이다. 자바 개발 키트 라는 이름 처럼 자바를 개발하는데 필요한

라이브러리들과 javac, javadoc 그리고 당연히 실행도 해야하기에 JRE 역시 포함 되어있다.

JDK 는 JRE 를 포함한 관계이다.

* <mark style="color:yellow;">**== 와 equals 의 차이점은 무엇입니까?**</mark>

String 생성에는 두가지 방법이 있다.

1. 리터럴을 이용한 생성
2. new 연산자를 이용한 생성

리터럴을 이용하게 되면 Heap 안에 있는String Constant Pool 에 저장되어지고

new 연산자를 이용하면 Heap 영역에 보관되어진다.

( Java 기본 타입은 Stack에 참조타입은 Heap 영역에 저장 된다.)

리터럴 생성 방식은 생성 시에 String Constant Pool 을 확인하여 해당 문자열이 존재하는지 확인하고 존재한다면 그 값을 꺼내주고 아니라면 생성 후 String Constant Pool 에 보관하는 방식이다.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

간단히 말하면 == 는 주소값을 비교 (어떤 객체를 참조하는지) , equals() 는 값을 비교한다.

new 생성자로 만들어진 동일한 문자열이 == 로 비교하니 false 라는 결과가 나왔다.

주소값을 비교하니 값은 같아도 new는 힙 영역 안에서 각기 다른 주소값을 가지고 저장되기에 해당 결과가 나온것이다.

하지만 리터럴 생성 방식은 해당 String 생성 시 String Constant Pool 존재하면 그 값을 주소값 까지 그대로 가져오기에 == 에서 true 값이 나온것이다.

equals() 는 값 자체를 비교하기에 생성방식 마다 차이 없이 값 만을 판단해 모두 true 값을 보여준다.

하지만 객체 자체를 비교할때는 = , equals() 둘다 동일하게 주소값을 비교한다.



\*추가

```
String a = "aa";
String b = new String("bb");

a = a + b;
```

위에  코드를 실행하면 각 "aa" , "bb" 에게 메모리가 할당되고 할당된 메모리의 주소를 a, b가 참조하게 된다.





* <mark style="color:yellow;">**두 객체가 동일한 hashCode를 가지면 Equals()가 참이어야 합니다, 그렇죠?**</mark>

정답은 맞다. 하지만 이유는 무엇일까?

동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, 동일한 객체는 동일한 해시코드를 가져야 한다.

hashcode() 는 해당 객체 주소를 가져와 해싱해서 해시코드 값으로 반환하는 메서드이다.

그렇기에 같은 객체주소면 같은 해시코드가 반환되야 한다.

_**(하지만 해시 충돌로 인해서 해시코드가 고유한 값은 아니라는것을 염두해두어야한다.)**_

그리 equals() 를 오버라이딩 할때 hashcode()도 같이 오버라이딩 해주지 않으면 오류가 발생한다.\\

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* <mark style="color:yellow;">**자바에서 final의 기능은 무엇입니까?**</mark>

final은 상수라고도 표현하며변수, 메서드, 클래스 에 붙일수있다. 어디에 사용하느냐에 따라 조금씩 기능이 달라지지만 어떤 초기값을 유지하고 변경하지 못하게 한다는 맥락에선 동일하다.

#### 변수

변수에 final 을 붙히게 되면 해당 변수는 수정 할 수 없다는 뜻이다.

그렇기에 초기화는 final 선언 과 동시에 필수이다.

#### 메서드

메서드에 final 을 붙히게 되면 해당 메서는 오버라이딩이 불가능 하다는 뜻이다.

#### 클래스

클래스에 final 이 붙게되면 상속이 불가능하다는 뜻이다.

* <mark style="color:yellow;">**자바에서 Math.round(-1.5)는 무엇을 의미합니까?**</mark>

round() 는 첫번째 자리까지 반올림해서 정수값을 리턴하는 함수이다.

* <mark style="color:yellow;">**String은 기본 데이터 타입입니까?**</mark>

아니다. String 은 클래스이다. 워낙에 자주 사용되기에 효율성이 높아야 하므로 클래스로 제공된다.

불변객체 이면서 String Constant Pool 이라는 캐시 기능도 가지고있다.

* <mark style="color:yellow;">**자바에서 문자열을 조작하는 클래스는 무엇이 있습니까? 각 클래스의 차이점은 뭘까요?**</mark>

String, StringBuffer, StringBuilder 가 있다.

String 은 불변객체로 변경이 되지 않는다. 만약 변경한다면 기존의 참조값은 Garbage로 남아있다.

GC에 의해 정리된다. 그래서 변하지 않는 값을 조회하는 기능이 많다면 String 을 사용하는것이 좋다.

하지만 추가, 수정, 삭제 등의 연산이 잦은 환경이라면 너무 많은 Garbage가 쌓이기에 성능 저하를 야기할수있다.

이러한 상황을 대처하기위해 StringBuffer, StringBuilder 가 등장햇다.

StringBuffer 는 가변성을 띄고있다. 수정이 불가능한 String 과 다르게 .append, .delete() 등의 메서드를 지원한다.

```
StringBuffer str1 = "abc";

str1.append("efg");

str1 = "abcefg"
```

위 코드 처럼 append 를 사용하면 String 처럼 Garbage 로 버리는게 아닌 그 주소값 그대로 문자열값만 변경되어진다. 그렇기에 추가, 수정, 삭제 등의 연산이 잦은 환경에선 String 보다 더 효율적이다.

StringBuilder 도 StringBuffer 동일하지만 동기화의 유무에서 차이가 갈린다.

StringBuilder는 동기화를 지원하지 않아서 단일 쓰레드에서의 성능이 더 좋다!

StringBuffer는 동기화를 지원해서 멀티 쓰레드 환경에 더 적합하다.

정리하자면 아래와 같다.

String : 문자열 연산이 적고 멀티쓰레드 환경일 경우

StringBuffer : 문자열 연산이 많고 멀티쓰레드 환경일 경우

StringBuilder : 문자열 연산이 많고 단일쓰레드이거나 동기화를 고려하지 않아도 되는 경우

* <mark style="color:yellow;">**String str ="i"와 String str = new String("i")가 동일합니까?**</mark>

\== 의 관점에선 동일하지 않고 .equals() 의 관점에선 동일하다고 생각한다.

* <mark style="color:yellow;">**문자열을 반전시키는 가장 좋은 방법은 무엇인가요?**</mark>

반복문을 돌리는 방법도 있지만 내가 생각하는 가장 이상적인 방법은 StringBuffer 의 .reverse() 메서드를 사용하는것 이라고 생각한다.

* <mark style="color:yellow;">**String 클래스의 일반적인 메서드는 무엇이 있나요?**</mark>

1. .length() : 해당 문자열의 길이를 반환한다.
2. subString( a, b) : a 부터 b 까지의 문자열을 잘라내서 반환한다.
3. charAt(index) : 인덱스 위치에 문자를 반환한다.
4. getByte() : 문자열을 바이트 형식으로 반환한다. 만약 파라미터로 인코딩 형식을 지정하면 해당 인코딩 방식으로 인코딩한 바이트 형식이 반환된다.
5. toLowerCase(), toUpperCase() : 전자는 소문자로 후자는 대문자로 변환해서 반환한다.

이 외에도 많은 메서드가 존재한다.

* <mark style="color:yellow;">**추상 클래스에서 추상 메서드는 필수적인가요?**</mark>

추상 클래스는 미완성된 클래스이다. 추상클래스 정의 자체가 추상메서드를 포함한 클래스 이기에 추상메서드는 필수적이다. 그렇지 않으면 일반 클래스와 다를게 없고 추상메서들 구현하지 않으면 오류가 발생한다.

* <mark style="color:yellow;">**보통의 클래스와 추상 클래스의 차이는 무엇인가요?**</mark>

추상 클래스는 클래스 명 앞에 abstract 가 붙지만 일반 클래스는 붙지 않는다.

추상 클래스는 몸체만 존재한다. 자식 클래스가 상속받아 구현해야하기 때문이다. 상속받아 구현하지 않으면 생성자 생성이 불가하다.

일반 클래스는 상속 하지 않아도 생성자 생성이 가능하다.

* <mark style="color:yellow;">**final은 추상 클래스를 수정할 때 사용할 수 있나요?**</mark>

불가능 하다.  다만 추상 클래스에 final 이 붙어있다면 해당 클래스에 메서드는 오버라이딩이 가능하다.





## \[동시성 이슈] HashMap vs ConcurrentHashMap
