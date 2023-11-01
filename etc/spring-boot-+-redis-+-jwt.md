# Spring Boot + Redis + Jwt

기존에 Spring Security + Jwt 를 사용해서 만든 웹서비스에 RefreshToken 을 사용하기 위해 Redis 를 사용해보았다.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

* 로그인에 성공하면 AccessToken, RefreshToken 을 발급한다. 이떼 RefreshToken은 Redis 를 통해 인메모리에  유저 ID 값을 키값으로 잡아 Token 값과 같이 저장하였다.
* 각 서비스를 이용할때 해당 AccessToken 을 사용해 인증, 인가를 진행한다.
* AccessToken 만료 시 RefreshToken 을 체크해서 유효하다면 AccessToken을 발급한다. 허나 RefreshToekn 도 만료라면 SecurityContextHolder에 Authentication 을 set 하지 않고 인증 인가를 허가 하지 않는다.



### RefreshToken

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

Spring Data Redis 에서 Redis 를 사용하는 방법은  크게 2가지 방법이 있다.



RedisTemplate 와 Redis Repository 이다.



필자는 후자인 RedisRepository 를 사용하였다. 두 방식 모두 사용전에 Redis 와연결 설정을 해야하는데 Redis 에서는 기본적으로 localhost 에 연결해주기 때문에 따로 설정없이 진행하였다.



<pre><code>public interface RefreshTokenRepository extends CrudRepository&#x3C;RefreshToken, String>{
<strong>}
</strong></code></pre>

마치 JPA Repository 설정하는것과 유사하다. 이렇게 설정하고 token을 조회할땐 @id 값에 해당하는 값을 통해 JPA 와 동일한 메서드를 통해 가져올수있다. 저장 역시 JPA 와 동일하게 save 메서드를 사용하면 된다.



<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

RefreshToken을 저장하면 해당 내용처럼 저장되어진다.



AccessToken의 유효기간은 짧게 RefreshToken은 다소 길게 잡아서 AccessToken 탈취 위험성을 낮춘다.  그리고 RefreshToken 을 이용하여 간단히 RefreshToken 을 삭제하는 로직으로 Logout 기능도 구현하였다.&#x20;

다만 Logout 시 AccessToken의 만료 기간이 아직 남아있다면 서비스에 접근이 가능하지 않나 라는 의문이 생겼다.&#x20;

어떤 글에서는 실제 서비스에서는 해당 액세스토큰의 탈취를 막기 위해 다양한 방법이 사용된다고 한다.  만약 탈취가 된다면유효기간을 최대한 짧게 잡는 방법 외엔 내가 알수잇는것은 없었다.&#x20;

**로그아웃 시 RefreshToken 을 삭제하고 AccessToken을 redis에 BlackLIst 로 등록해 해당 리스트에 엑세스 토큰이 존재하면 만료된것으로 인지하도록 로직을 작성했지만 Stateless 를 지키지 못한다는 점에서 아쉬움이 남는다.**

```
// 로그아웃 로직

 public void logOut(HttpServletRequest request){
        String refreshToken = request.getHeader("refreshToken");
        String accessToken = request.getHeader("accessToken");

        if (refreshToken != null && validateRefreshToken(refreshToken)){
            String idByToken = getIdByToken(refreshToken);
            refreshTokenRepository.deleteById(idByToken);
            
            if (validateAccessToken(accessToken)){
                BlackListTokenDto blackListTokenDto = BlackListTokenDto.builder()
                        .token(accessToken)
                        .memberId(getIdByToken(accessToken))
                        .build();
                BlackListToken blackListToken = blackListTokenDto.toDto(blackListTokenDto);

                blackListTokenRepository.save(blackListToken);


            }
        }
        
/// 엑세스 토큰 검증 시 blackList 해당 여부 확인 로직 추가


public boolean validateAccessToken(String accessToken) {
        if(validateBlackListToken(accessToken)){
            log.info("해당 토큰은 유효하지 않습니다.");
            return false;
        }

        try {
            SecretKey key = Keys.hmacShaKeyFor(secretKey.getBytes(StandardCharsets.UTF_8));
            Jws<Claims> claims = Jwts.parserBuilder().setSigningKey(key).build().parseClaimsJws(accessToken);

            return !claims.getBody().getExpiration().before(new Date());

        } catch (Exception e) {

            return false;
        }
    }
```



## 정리

RefreshToken을 사용 함으로써 기존 AccessToken 만을 사용하였을때의 토큰 탈취 위험성을 낮출수있었다. 문제가 생기면  RefreshToken 을 삭제 시키면 되기에 Logout 기능도 구현하였다.

다만 로그아웃 시 AccessToken의 유효기간이 남아있다면 로그아웃 후 에도 서비스 접근이 가능하다는 점에서 위 같은 BlackList 등록 같은 로직을 사용했지만 StateLess 함을 유지하지 못햇다는 점에서 아쉬움 이 남는다.

요새는 많은 회사들이 클라우드 서비스를 사용하는데 주로 스케일 아웃 방식을 선호한다고 알고있다.

Stateless 한 방식을 사용하는 이유에도 스케일 아웃 방식에 Fit 한 방식이라는 점을 빼놓을수 없을거같다.











































