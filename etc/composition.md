# 상속 보단 조합(Composition)

상속 보단 조합 or 합성 (Composition)

상속은 재사용성 보단 오히려 확장 쪽에 더 가깝고 이점이 많다.

상속을 한다는건 자체가 명시적이고 캡슐화를 깨는 행동이고 상속을 하는, 상속 되어지는 양 쪽이 강하게 결합된다.

그리고 컴파일 단계에서 의존성을 해결할 수  있다.

그러기에 상속은 완벽한 is-a 관계거나 기존 API 를 수정해도 상위 클래스의 변화가 하위 상속 클래스에 영향이 없는 경우가 아니면 사용을 자제해야된다.



조합은 has-a 관계이다.

조합은 메서드를 호출 하는 방식이기 때문에 캡슐화를 깨지않는다.

그리고 런타임 단계때 의존성을 해결할 수 있다.

클래스가 안에 내용이 변경되더라도 메서드를 호출하기에 내부 변화가 거의 없다.

단순히 변수로 상태를 변화시키는것 보단 명확한 행동을 기준으로 상태를변경한다면 훨씬 재사용성도 높아지고 결합도도 낮출수있다.