# ISSUE

CASE 1  : SPRING BOOT ,  JPA  프로젝트 에서 이미 존재하는 오라클 디비 와 엔티티 매칭 하는 로직

기존에는 따로 디비를 만들지 않고 프로젝트 안에서 기재한 엔티티를 바탕으로 DB 테이블을 생성해서 사용하여서 해당 로직은 처음 접해봤다.



* 도메인 파트

@Entity : 필수이다. 해당 클래스가 객체이며 DBMS 와 연결해야 한다는 사인이므로!

@Getter : 스프링 안에서 구동될때 get 이 없으면 오류가 나는걸로 알기에 넣었다.

@NoArgsConstructor : 생성자 생성을 위해서 삽입

@Immutable : 오라클의 뷰테이블을 매칭 하는것인데 뷰테이블 이기도 하고 해당 데이터는 read-only 이기 때문에 삽입

@Table : 아주 중요하다. 어떤 테이블에 매칭할것인지 명시해야하는 어노테이션이다. 처음에 테이블 명을 잘 못 명시해 오류가 났다.

@Id : JPA 에서 객체는 무조건적으로 유니크값 즉 @Id 를 가지고 명시해야한다.

처음에 GRP\_CD 를  ID 로 잡고 출력하니 동일한 CD 를 가진 컬럼은 컬럼값이 CD 를 가진 여러 컬럼 중 첫 번째 컬럼값으로만 출력되는 오류를 냈다. 고로 유니크값을 ID 로 잡아서 출력하니 정상 출력 되었다.





하지만 유니크값인줄 알았던 값이 알고보니 유니크값이 아니였고 테이블에 있는 어떤 컬럼도 UNIQUE 하지 않았다. 그래서 한 로우 자체를 즉 모든 컬럼을 묶어서 복합키로 만들어서 복합키에 PK 값을 지정해주었다.



혹은 SEQUENCE\_GENERATOR 를 이용하여 임의의 시퀀스값을 만들어 해당 값을 PK 로 지정하였다.



이때 오라클에서 컬럼을 생성한 뒤 해당 컬럼의 이름으로 시퀀스를 따로 생성해주어야한다.

```
CREATE SEQUENCE your_sequence_name
    START WITH 1
    INCREMENT BY 1
    MINVALUE 1
    MAXVALUE 999999999999999999999999999
    CACHE 20;
```







1. @SpringBootApplication(exclude={DataSourceAutoConfiguration.class}) 삭제 시 hibernate-dialect 오류 발생

* spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.OracleDialect (해당 applicaiton.properties에 주입)



2. 오라클 DB 연결 설정 정보 오류&#x20;

* 마지막 서버이름 부분 SID 일시 에만 :SID,   /서버이름
* spring.datasource.url=jdbc:oracle:thin:@DB IP 주소:SID
* spring.datasource.url=jdbc:oracle:thin:@DB IP 주소/서버이름



```
@Getter
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Immutable
@Table(name = "DB 테이블명")

public class   {

    @Column(name = "")
    private String ;
    @Id
    @Column(name = "" , columnDefinition = "VARCHAR2(100)")
    private String ;

    @Column(name = "" , columnDefinition = "VARCHAR2(100)")
    private String ;

    @Column(name = "" ,columnDefinition = "NUMBER")
    private String ;

}
```





원격 WAS 설정 시 유의사항

1. JDK 버젼
2. sever.xml - docbase 경로, db 경로
3. 톰캣 환경변수
4. 자바 환경변수
5. 방화벽 여부





was 실행 시 히카리풀 오류 &#x20;



* 디비 커넥션 오류





분기, 리턴값 타입  신경쓰자



생성자를 통해 일종의 인터페이스 처럼 틀을 잡을수있다.&#x20;

예외처리, 변수명,  중복코드제거, &#x20;



1. 예외처리
2. 서비스에서 다른 서비스 호출
3. 컨트롤러의 관심 범위
4. URL 경로 분기 설계
5. 컨트롤러 파라미터 DTO 로 전환
6. 개발 전 API 명세서 작성의 중요성

