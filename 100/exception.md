# Exception

## <mark style="color:yellow;">Throw와 Throws의 차이는 무엇인가요?</mark>

&#x20;**Throws**

메서드에서 잠재적으로 어떤 Exception 이 발생 할 수 있는지 명시할때 사용한다.



**Throw**&#x20;

Exception을 발생 시킬 때 사용한다.



ex.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:yellow;">Final, Finally, Finalise의 차이는 무엇인가요?</mark>



**final**&#x20;

클래스, 메서드, 필드가  수정이 불가하도록 설정할때 사용한다.



**finally**&#x20;

try catch 문 사용 시에  Exception이 발생하든 안하든 실행되는 동작을 설정할때 사용한다.



**finalise**

소멸자 메서드 라 칭하며 GC가 사용하지 않는 배열, 객체 등을 힙 영역에서 삭제하는 메서드이다.



## <mark style="color:yellow;">Try-Catch-Finally에서 생략할 수 있는 부분이 무엇인가요?</mark>



Finally 는 필수가 아니므로 생략이 가능하다.

try - finally 사용 시 catch 문 생략이 가능하다.



## <mark style="color:yellow;">Catch가 반환되면 finally가 실행되나요?</mark>



**finally 실행 조건**&#x20;

예외가 없을 때 'try' - 'finally' 순으로 실행

예외가 있을 때 'try' - 'catch' - 'finally' 순으로 실행



## <mark style="color:yellow;">Exception 클래스의 예시를 말해주세요.</mark>



1. RuntimeException
2. 그 외의 Exception 클래스의 자식 클래스

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>





































