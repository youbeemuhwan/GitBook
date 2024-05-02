# 디미터 법칙 (클린코드)

### 디미터 법칙

모듈은 자신이 조작하는 객체에 속사정(자료)를 몰라야 한다는 법칙이다.

설명만 봤을땐 감이 안왔는데 쉽게 풀이하면 여러개 .(dot) 의 사용을 자제하라 라는 뉘앙스로 받아들였다.

어떠한 객체가 다른 객체에 대해 너무 많이 알고있으면 결합도가 높아져 객체지향이라는 개념과 거리가 멀어지기 때문이다.

디미터 법칙을 준수하여 캡슐화를 높이고 객체의 자율성과 응집도를 높이는데 초점이 맞춰져 있는 법칙이다.



```
@Getter
public class Member{

    private final String name
    private final orderInfo orderInfo

    public String getOrderInfoPostalCode(){
        return this.orderInfo.getAddressPostalCode();
    }
}

@Getter
public class OrderInfo{

    private final String orderNumber
    private final String address

    public String getAddressPostalCode(){
        return this.address.getPostalCode();
    }

}

@Getter
public class Address{

    private final String postalCode
    private final String city
    private final String etc
}
```





```
public class order{

    public static void main(String[] args) {

        final Address address = new Address("435-010","경기도 군포시","당동로 75");
        final OrderInfo orderInfo = new Address("B102010212", address);
        final Member = new Address("김아무개", orderInfo);

        // 1번 : 디미터 법칙 위반 코드
        System.out.println(member.getOrderInfo().getAddress().getPostalCode());

        // 2번 : 디미터 법칙 준수 코드
        System.out.println(member.getOrderInfoPostalCode());
    }
}
```



본인도 디미터 법칙을 알기 전 에는 1번과 2번 코드가 뒤섞여서 작성하였다.

당시에는 1번이 좀 더 가독성이 높다고 판단했다. 객체의 값들을 모두 볼수있으니 어떻게 동작되는지&#x20;

더 명확히 볼수있단 생각에서 였다.&#x20;

하지만 지금 보니 1번 코드도 타인이 코드를 살필 땐 어차피 각 객체가 어떻게 구성되있는지를 체크해야되기 때문에 가독성 면에서는 큰 이점을 보기는 어렵다고 생각을 바꾸게 되었다.

&#x20;2번 코드는  각 함수라는 메시지를 통해 자료를 제공받기에 객체는 객체의 속사정을 알기 어려워져 디미터 법칙을 준수한 코드가 된다.&#x20;

그렇기에  2번코드는 코드의 깔끔함 과 객체지향적 이점을 모두 챙길수있다.





다만 예외적인 부분도 있다.

DTO, 자료구조, Method Chaining 을 사용하는 경우 디미터 법칙을 위반하지 않는다.

그렇기에 너무 .(dot) 을 붙히면 안된다는 것에 초점을 맞춰선 안된다.
