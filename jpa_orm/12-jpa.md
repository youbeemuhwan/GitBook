# 12장 스프링 데이터 JPA

스프링 데이터 JPA 는 JPA를 편리하게 사용할수 있도록 도와주는 모듈이다.

스프링 데이터 JPA 의 Repository 구현으로 개발자가 일일히 EntityManager 를 명시하지 않아도 내부적으로 EntityManager  를 사용해   JPA를 편하게 사용할수있게 해준다.



<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## 쿼리 메소드 기능

1. 메소드 이름으로 쿼리 생성
2. 메소드 이름으로  JPA NamedQuery 호출
3. @Query , Repository 메서드에 쿼리 직접 입력

필자는 간단한 find 는 1번을 사용하였고 그 외에는 3번 혹은 QueryDsl 을 사용했다.

## 파라미터 바인딩

```
select m from Member m where m.username = ?1 //위치 기반
select m from Member m where m.username = :name //이름기반
```

## 페이징과 정렬

Page 를 사용하면 스프링 데이터 JPA가 검색된 전체 데이터 건수를 조회하는 count 쿼리를 추가로 날린다.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

자신이 쓴 게시글 리스트를 가져오는 기능인데 첫번째 SELECT 문 과 더불어 두번째 SELECT COUNT 쿼리가 날라간걸 볼수있다.



해당 카운트를 이용하여 페이징을 구성한다.
