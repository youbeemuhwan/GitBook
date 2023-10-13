# 동기 / 비동기 | Blocking / Non-Blocking

동기

주어진 일들을 _**순서대로**_ 처리하는것을 의미한다.

밑에 사진 처럼 배당된 작업이 끝나지 않으면 다음 순서의 작업으로 넘어가지 않는것이다.



<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

비동기

주어진 일들을 _**순서를 보장하지 않고**_ 처리하는 것을 의미한다.



배당된 작업 순서대로 작업을 처리하는것이 아닌 작업 처리 중에도 다른 작업을 같이 처리하여 멀티로 작업을 처리하여 처리량에 있어 효율을 높인 방식이다. 하지만 중요한건 순서를 보장하지 않는다는것!



<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

Blocking

일련의 작업들을 _**하나 하나 씩**_ 처리하는 방식.

하나의 작업이 시작되면 다른 작업은 Block 처리 되어 작업이 끝날때까지 수행될수 없다.

Non-Blocking

일련의 작업들을 _**병렬 형식**_ 으로 같이 처리하는 방식

하나의 작업이 끝나지도 않아도 Non-Block 이기에 다른 작업이.수행될수있어서 동시에 작업이 수행 가능 하다.



동기 / 비동기 , Blocking / Non-Blocking &#x20;

두 개념 모두 비슷해 보이지만 중점으로 내용이 다르다.

동기 / 비동기 는 순서가 중점이고 Blocking / Non-Blocking 는 현재 작업이 block(차단, 대기) 되느냐 아니냐 가 중점이다.

