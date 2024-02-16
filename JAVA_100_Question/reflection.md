# Reflection

## <mark style="color:yellow;">Reflection 이란?</mark>

JVM 은 클래스 로더를 통해 클래스 정보를 읽어와서 JVM 메모리에 저장한다.

클래스를 거울에 비친거처럼 클래스 정보를 가지고있어 리플렉션이라는 이름을 가지게되었다.

클래스에 세세한 정보들 까지 가지고있고 이것을 참조해 다양한 어노테이션 들이 동작할수있다.

### 사용 예시

* IntelliJ의 자동완성 기능
* Lombok 에 Getter, Setter
* Spring Annotation

## <mark style="color:yellow;">자바 직렬화란 무엇인가요? 어떤 상황에서 필요한가요?</mark>

직렬화는 자바에서 사용되는 객체, 데이터들을 바이트스트림 형태로 변환하는 포맷 변환 기술이다.

JSON / XML 직렬화

BINARY 직렬화

JAVA 직렬화 (사용하지 않는걸 권고)

### **장점**

1. 자바에 다양한 객체, 클래스, 데이터 타입 과 같은 레퍼런스 타입에 대해 제약없이 내보낼수있다.
2. 자바 시스템 개발에 최적화 되어있다.

### 사용처

#### 캐시

데이터가 필요할때 DB에 접근하는것이 아닌 직렬화 한뒤 저장해놓고 필요할때 역직렬화 하여 사용할수있다. 하지만 요즘엔 캐시 DB 를 많이 사용한다. ex. Redis

#### 서블릿세션

세션을 단순히 서블릿 위에 올려놓고 사용한다면 필요치 않지만 세션데이터 저장, 공유를 한다면 필요로 한다.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

보통 요즘은 JSON 으로 많이 데이터를 주고 받지만 직렬화가 가진 장단점을 잘 고려해서 JSON 과 저울질 하여 직렬화가 낫다고 판단되면 사용하면된다. 다만 주의할 사항이 많기에 왠만하면 JSON 으로 내보내는것이 바람직하다고 생각한다.



## <mark style="color:yellow;">동적 프록시란 무엇인가요?</mark>

개발자가 직접 만드는게 아닌  인터페이스 또는 클래스를 기반으로 동적으로 만들어주는 프록시를 뜻한다.

정적 프록시는 만드는것 자체는 어렵지 않을수있지만 각 타겟 클래스를 알아야하고 각 타겟 클래스 별 로 일일히 프록시를 만들어줘야 한다.&#x20;

단순시 트랜잭션 프록시를 만들어도 동일한 코드를 각 타겟 클래스 별로 만들어야 하기에 수고스러움이 있다.&#x20;

이러한 불편함을 해결하러 나온것이 동적 프록시 이다.

클래스 또는 인터페이스 기반으로 동적으로 만들어줘서 기존의 정적 프록시가 가지고있는 단점을 해결하였다.



## <mark style="color:yellow;">동적 프록시는 어떻게 사용하나요?</mark>

자바에서 동적프록시는 리플렉션 패키지에 포함되어있어 따로 의존성 주입을 안해줘도 된다.

InvocationHandler 라는 인터페이스에  invoke 라는 메서드를 구현해줘야한다.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

해당 인터페이스를 구현한 뒤 사용 하려면 해당 핸들러를 생성 한 뒤 Proxy.newProxyInstance() 객체를 생성한다. 해당 객체에는 타겟 메서드를 지정해야 한다. 만약 타겟 메서드는 인터페이스 여야 한다. 그렇지 않으면 오류가 발생한다.



```
@Test
void save_normal_member() {
    String normalMemberName = "PARKER";
    MemberRepository target = new NormalMemberRepository();

    TransactionInvocationHandler handler = new TransactionInvocationHandler(target);
    MemberRepository proxy = (MemberRepository) Proxy.newProxyInstance(
            MemberRepository.class.getClassLoader(), new Class[]{MemberRepository.class}, handler);

    log.info("targetClass={}", target.getClass());
    log.info("proxyClass={}", proxy.getClass());
    proxy.save(normalMemberName);
}
```

```
@Test
void save_admin_member() {
    String adminMemberName = "PARKER_ADMIN";
    MemberRepository target = new AdminMemberRepository();

    TransactionInvocationHandler handler = new TransactionInvocationHandler(target);
    MemberRepository proxy = (MemberRepository) Proxy.newProxyInstance(
            MemberRepository.class.getClassLoader(), new Class[]{MemberRepository.class}, handler);

    log.info("targetClass={}", target.getClass());
    log.info("proxyClass={}", proxy.getClass());
    proxy.save(adminMemberName);
}
```

참조 : [https://velog.io/@codemcd/%EB%8F%99%EC%A0%81-%ED%94%84%EB%A1%9D%EC%8B%9CDynamic-Proxy-with-Spring](https://velog.io/@codemcd/%EB%8F%99%EC%A0%81-%ED%94%84%EB%A1%9D%EC%8B%9CDynamic-Proxy-with-Spring)



만약 인터페이스가 아닌 클래스를 타겟으로 동적 프록시를 생성하고 싶다면  CGLIB 를 사용하면 된다.





