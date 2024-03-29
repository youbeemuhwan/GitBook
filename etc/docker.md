# Docker

도커는 컨테이너 기반의 오픈소스 가상화 플랫폼 이다.

요즘 백엔드 신입이 가져야할 기본소양에 도커, 쿠버네티스 배포 경험을 이야기하는것을 꽤나 많이 봤을 정도로 많은 사람들이 사용하고있다.&#x20;



도커에 대해 알기 전에 일단 가상화에 대해 알아야 했고 가상화에 대해 알려하니 기본적인 컴퓨터 3계층에 대해 알아야 했다.



가상화는 직접 해봤음에도 역시 가상화 설정법을 보면서 따라가기만한 덕에 흐름을 이해하는데 꽤나 시간이 들었다.

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>



일단 위에 사진을 바탕으로 설명하자면 컴퓨터는 크게 두가지로 나뉜다.

* 하드웨어 (H/W)
* 소프트웨어 (S/W)

그 중 소프트웨어는 또 두가지로 나뉜다.

* 애플리케이션
* &#x20;커널

애플리케이션은 우리가 흔히 아는 프로그램을 말한다.&#x20;

커널은 OS 의 핵심 부분으로서 자원을 관리하는 역할을 담당한다. 우리가 흔히 터미널 이라고 말하는 애플리케이션이 커널 과 사용자 간의 소통을 도와주는 애플리케이션이다.



### &#x20;컨테이너 기반으로 까지 의 발전 과정

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

가상화는 하나의 하드웨어 위에 하나 이상의 OS 를 운영하는것을 의미한다.

하나의 NIC 즉 하나의 하드웨어로 여러 OS 를 운용하며 네트워킹 까지 가능하게 한다.

어떻게 가능한것일까?

가상화 소프트웨어는 기존 호스트OS 위에 설치된다. 그러면 하이퍼바이져 라는 프로그램을 이용하여 기존 OS를 이용하여 가상 머신 위에 올라간 게스트 OS 에게 할당되는 자원을 관리한다.

마치 호스트 OS 의 자원을 관리하는 커널 처럼 각 가상 머신위에 올라간 게스트 OS 의 자원은 하이퍼바이져가 관리한다. &#x20;

가상화 머신은 모두 소프트웨어로 이루어져있다. NIC 역시 NIC 드라이버라는 소프트웨어가 하드웨어 역할을 할뿐.  이러한 소프트웨어들 덕분에 마치 여러개의 컴퓨터를 사용하듯 자연스레 각각의 OS 가 동작하고 네트워킹도 가능하게끔 해주는것이다.

이 부분을 인지하지 못한채로 가상화 계층 이미지를 바라봤을땐 왜 도커가 그렇게 유용한지 어떤 생각으로 도커가 만들어졌는지 이해하기 어려웠기에 해당 부분을 찾아볼수밖에 없었다.



또 이해하기 어려웠던 부분은 도커에선 어떻게 OS 를 관리하는가 였다.

HOST OS 만 존재하고 게스트 OS 가 없는 부분을 이해하기 쉽지않았다.

도커는 기존에 가상화에서 각각 머신마다 갖고있는 게스트 OS 들에 대해 도커라는 애플리케이션을 통해 호스트 OS의 기능을 가져와 기존 게스트 OS 같은 격리된 환경을 제공해준다.

그렇기에 따로 게스트 OS가 필요하지 않은것이다. 물론 기존 게스트 OS 만큼 정밀하지는 않을거같지만

마치 호스트 OS를 여러번 실행하는것 이라는 비유를 듣고 단번에 이해됬다.

한글을 설치하고 한글을 여러번 실행해서 여러 창을 띄우듯 호스트 OS 역시 같은 개념으로 다가가 게스트 OS 를 대신해주는것이다.

### 도커 데스크탑

기본적으로 도커는 리눅스 환경에서 동작한다.  호스트 OS가 리눅스가 아니라면 도커 데스크탑을 설치해야 한다. 주로 맥이나 윈도우에서는 말이다. 도커 데스크탑은 호스트 OS 를 이용해 리눅스 기반의 가상환경을 구축하는데 도움을 주는 애플리케이션이다. 윈도우나  MAC 사용 시 필수로 설치해야한다.



기존에 프로그램을 배포할때 기존에 개발했던 곳에 환경설정, OS, 라이브러리 등등 모두 컨테이너에 담아 그대로 도커로 띄우기만 하면 일일히 설정할 필요도 없고 환경에 관련된 오류를 마주할 일도 없이 빠르게 배포가 가능하다. &#x20;

도커 이미지라는 것이 있는데 난 이것을 설치파일로 비유하고싶다.

이 도커 이미지를 도커에 띄우면 하나의 컨테이너가 설치되는것이다. 이 컨테이너 안에는 위에 말한것들이 모두 담겨있다.  이미지 와 컨테이너는 1:다 관계 이다.

또한 이미지의 용량이 클것인데 그것도 도커허브에 올려놓으면 어디든 도커 허브에서 실행만 하면 바로 배포가 가능하다.&#x20;

또한 이미지 레이어 방식으로 기존 이미지에서 파일 하나를 추가했다고 새로운 이미지를 다운받기는 번거롭기 때문에 이미지를 레이어 하는 방식으로 새로운 레이어 하나만 받으면 해당 추가사항을 반영할수있게 하였다.

이렇게 이해하고나니 왜 도커가 인기인지 많은 사람들 입에서 오르내리는지 단번에 이해됬다.



번외로 쿠버네티스는 도커를 관리해주는 역할을 해준다. 하나의 도커만 사용하면 모르지만 여러개의 도커를 사용하면 그것을 관리하는것도 만만치 않은 일이다. 그렇기에 그것을 관리해주는 쿠버네티스 역시 도커와 같이 많이 언급되어진다. 다만 도커 지원을 중단한다는 글을 보기는 했는데 기존 도커는 게속 사용이 가능하다고는 하는데 자세한건 내가 직접 도커를 사용해봐야 알수있을거같다.





