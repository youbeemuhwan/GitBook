# 서버에 WAS 패키징 시 오류

Caused by: oracle.net.ns.NetException: The Network Adapter could not establish the connection oracle.net.nt.ConnStrategy.execute(ConnStrategy.java:715) at oracle.net.resolver.AddrResolution.resolveAndExecute(AddrResolution.java:584) at oracle.net.ns.NSProtocol.establishConnection(NSProtocol.java:964) at oracle.net.ns.NSProtocol.connect(NSProtocol.java:350) at oracle.jdbc.driver.T4CConnection.connect(T4CConnection.java:2441) at oracle.jdbc.driver.T4CConnection.logon(T4CConnection.java:656) ... 87 more Caused by: java.io.IOException: Connection timed out: connect, socket connect lapse 21001 ms. ip



내부망에 있는 서버들은 모두 구동이 잘되는데 내부망 밖에 있는 서버만 was에서 패키징 하면 tomcat 에서 해당 오류를 내뱉으면 히카리풀 로딩에서 앞으로 넘어가질 못한다.&#x20;



결국 셧다운 되버린다.

처음엔 실행이 되는 서버, 안되는 서버의 내부망 연결유무를 알기 전에는 단순 스프링 설정 , 서버 설정 문제 인줄 알았다. 복합키 엔티티가 저장 시 오류가 뜨나? 했다

하지만 자세히 보니 로그에 해당 오류가 함께 뜨고 내부망안에 없는 PC 에서는 빌드는 되지만 해당 오류 또한 같이 뱉어지는 상황을보고 해당 디비 연결에 문제가 있다고 판단했다.



결국은  연결할려는 디비가 내부망에서만 접속 가능한 디비이다 보니 내부망 밖에서는 접속이 안되니 해당 에러가 뜨는것은 당연한 상황이였다,,

기존에 연결한 내부망 DB 말고 현재 로컬에 있는 디비로 변경한 뒤 ddl auto 설정을 create 로 해서 해당 디비의 기존에 오라클 디비에 연결하려고 만들어놓은 엔티티가 그대로 생성되게 설정 하였다.&#x20;





\






