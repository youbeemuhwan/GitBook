# 한 줄 요약

* 객체 간에 의존성을 아예 배제 할순없다. 다만 의존성의 유의미한 명분을 통해 적절한 캡슐화를 적용하여 결합도를 관리하는것이 중요하다.



* 설계 단계에서 클래스에 집중하는것이 아닌 어떤 속성과 메서드가 필요한지 고민 한뒤 거기에 맞춰 클래스를 잡아야한다.&#x20;



* (클래스의 윤곽을 잡기 위해서는 어떤 객체들이 어떤 상태와 행동을 가지는지를 먼저 결정해야 한다.)



* 결국 클래스란 어떤 객체들이 어떤 상태와 행동을 가지는지에 대한 기준을 토대로 공통분모를 잡아 추상화 한것이기 때문이다.



* 객체를 단순히 하나의 독립체로 보기보단 각각 객체가 서로 협력하는 공통체 일원으로 봐야한다. 훌륭한 협력이 훌륭한 객체를 낳고 훌륭한 객체가 훌륭한 클래스를 낳는다.



* 객체의 책임은 무엇을 알고있는가, 무엇을 할수있는가 로 구분할수있다.



* 애플리케이션 에서의 객체의 존재 이유는 어떤 협력에 참여하고 있고 그 협력에 필요한 것을 보유하고있기 때문이다.



* 객체에 있어서 구현이 아닌 책임에 집중하자