# Redis 란?

궁금해서 몇번 찾아보고 프로젝트에 적용 시켜볼려고 시도도 했었는데 막상 출근길에 그래서 Redis가 뭔지 일목요연하게 설명을 못하겠다는 생각이 들어 정리해본다.



\*인메모리 데이터 구조&#x20;

일반적인 DB는 디스크 즉  DB에 있는 저장소에 저장을 하는데 반면 인메모리 구조는 컴퓨터 메모리에 저장하는 방식의 DB이다. 디스크에 접근하려면 쿼리를 날려서 접근해야 하고 시간, 비용이 비교적많이 든다.

하지만 인메모리 구조는 메모리에 저장하기에 접근이 쉽고 빠르다. 단점으론 휘발성이 강하다는 점 과

메모리 크기가 부족할 경우 가상메모리를 사용해서 속도가 많이 저하 된다는 점이 있다.

그렇기에 캐시서버로 주로 이용된다. ex. 세션 정보



## Redis

&#x20;Key : Value 형태를 지닌 비관계형 데이터베이스 이다. 인메모리 데이터 구조를 가지고있다.\
주로 캐시 서버로 이용되며 DBMS 와 같이 사용되는 경우가 많다.&#x20;





### Look aside 패턴

마치 String Constant Pool 의 동작방식과 거진 똑같다.

웹사이트에서 데이터를  요청을 하면 해당 데이터가 캐시서버에 있는지 확인 한다. 있다면 반환하고 없다면 디비에 접근 후 데이터를 가져와 캐시서버에 저장하는 방식이다.



### Write Back 패턴

웹서버 의 모든 데이터를 캐시 서버에 저장하는 방식이다.

일정 시간동안 데이터를 캐시 서버에 저장한다. 그리고 캐시서버에 저장된 데이터를 DB에 저장한다.

DB에 저장된 캐시서버 데이터는 삭제한다.



500개의 쿼리를 일일히 하나씩날리는것보다 모아서 한번에 날리는것이 효율적이다 라는 판단에서 만들어진 패턴이다. 다만 인메모리 데이터 구조 특성 상 휘발성이 강해 오류가 생기면 데이터가 다 날라가기에 주의 해야 한다.



## 주의할 점

1. 휘발성이기 때문에 그에 대한 대비책이 마련되있어야 한다.
2. &#x20;위에도 작성했듯이 메모리 관리가 중요하다. 가상메모리 사용 시 속도가 저하 되므로
3. 싱글 스레드를 사용하기 때문에  처리 시간이 오래걸리는 요청은 자제하는것이 좋다. &#x20;



Redis 의 장점과 단점을 살펴보면 왜 Redis 가 캐시서버로 사용되는지 명확히 알수있었다.

