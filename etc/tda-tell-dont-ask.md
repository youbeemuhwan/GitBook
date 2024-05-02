# TDA (Tell, Don't Ask) 법칙

TDA 원칙 (Tell, Don't Ask)

전에 작성한 디미터 법칙과 맥락이 비슷한 원칙이다.

직역하면 묻지말고, 말하라 인데 객체의 내부 정보를 묻지말고 로직을 시키라는 원칙이다.



<pre><code>// TDA 법칙을 지키지 못한 코드

@Getter
public class Member {

    private final String email
    private final String age

}

public APP {

    Member member = new Member();
    
    isValid(member.age ,member.email);

public Boolean isValid(String age, String email){

        if(age &#x3C; "20" &#x26;&#x26; email.endsWith("naver.com")){
            return true
        }
        
        else{
            return false;
        }
<strong>    }
</strong>}

</code></pre>





<pre><code>// TDA 법칙을 준수한 코드
<strong>
</strong><strong>@Getter
</strong>public class Member{

    private final String email
    private final String age


public Boolean isValid(){
    if(this.age > "20" &#x26;&#x26; this.email.endsWith("naver.com"){
        retrun;
        }
    }
}

public APP{

    Member member = new Member();
    
    if(!isValid()){
        throw new RuntimeException("검증에 실패했습니다.")
    }
}
</code></pre>

일일히 객체에게 Getter 로 정보를 가져와서 로직을 외부에서 구현하지말고 시키기만 하고

해당 로직은 자기 자신의 정보로 그 객체가 로직을 내부에서 구현해야한다는 뜻이다.



1번은 Member 객체가 자신의 정보를 검증하는 함수를 비즈니스 로직에 적어놓았다.

그렇기에 해당 Member 의 정보를 가져온뒤 파라미터로 옮겨줘야한다.  뭔가 행동을 시킬때 아무것도 묻지말라는 TDA 법칙에 어긋나게 된다.



2번은 Member 자신의 정보를 검증하는 로직을 자기 쪽에 직접 함수를 선언해 해당 member의 정보를 누구에게 묻지않아도 로직을 진행할수있다. 아무것도 묻지않고 행동을 시킬 수 있으므로 TDA 원칙을 준수하는 코드이다.



다만 객체와 객체 사이의 규칙이기 때문에 디미터 법칙 게시글에도 말했듯이 자료구조에는 크게 해당 되지않는 부분이다.

물론 DTO 를 작성할때 Entity 를 그대로 가져가 사용한다면 함수화 하여 사용하면 더 깔끔하겠지만

모든 DTO가 구성이 위 같이 구성되지는 않기에 코드, 상황에 따라 적절하게 적용하는것이 좋다고 생각한다.
