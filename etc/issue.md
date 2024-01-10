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

