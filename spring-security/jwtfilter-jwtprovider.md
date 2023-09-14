# JwtFilter, JwtProvider

## JwtFilter

JwtFilter 는 해당 OncePerRequestFilter 를 확장시켜 각 요청마다 토큰의 유효성을 검증한다.

스프링시큐리티가 제공하는 formLogin 을 사용한다면 UsernamePasswordAuthenticationFilter 필터가 기본적용되서 form 형식에서 온 아이디, 패스워드 값을 받아 유효한 회원인지 패스워드가 일치하는지 체크하고 세션에 세션쿠키를 사용해 사용자의 권한이나 인가 유무를 구분한다.

하지만 Jwt 사용시 세션을 사용하지 않다는 점과 로그인 과정이 유효하다면 Jwt 토큰을 따로 발급해야하는 로직이 필요하기에 formLogin을 사용하지 않는다. 그렇기에 UsernamePasswordAuthenticationFilter 앞 단에 따로 만든 CustomFilter 를 통해 토큰 유효성을 체크해야한다.

<pre><code>@RequiredArgsConstructor 
public class JwtFilter extends OncePerRequestFilter {
<strong>
</strong><strong>private final JwtProvider jwtProvider;
</strong>
@Override
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
    String token = jwtProvider.resolveToken(request);

    if (token != null &#x26;&#x26; jwtProvider.validateToken(token)) {
        Authentication auth = jwtProvider.getAuthentication(token);
        SecurityContextHolder.getContext().setAuthentication(auth);
    }
    filterChain.doFilter(request, response);
}
}
</code></pre>

filter 를 만드는 법은 따로 정해진건 없고 원하는 로직에 따라 각자 다르다. 필자는 요청 당 딱 한번만 실행되는 OncePerRequestFilter 를 확장시켜 해당 doFilterInternal 메서드를 오버라이드 했다.



**OncePerRequestFilter 이란**

* 모든 서블릿에 일관된 요청을 하기위한 필터, 요청 한번 당 한번만 실행되기에 스프링 시큐리티에서 요청 마다 토큰을 검증하는 로직에 사용된다.

여기서 주입된 JwtProvider 를 살펴보자



### JwtProvider

<pre><code>@RequiredArgsConstructor 
@Component 
public class JwtProvider {
private final UserDetailService userDetailService;
private final Long TokenValidTime = 1000L * 60 * 60;

@Value("$(jwt.secretKey)")
private String secretKey;

public String createToken(Long id, String email){
    Claims claims = Jwts.claims().setSubject(email);
    claims.put("id", id);
    Date now = new Date();
    return Jwts.builder()
            .setClaims(claims)
            .setIssuedAt(now)
            .setExpiration(new Date(now.getTime() + TokenValidTime))
            .signWith(SignatureAlgorithm.HS256, secretKey)
            .compact();
}

public Authentication getAuthentication(String token){
    String subject = getEmail(token);
    UserDetails userDetails = userDetailService.loadUserByUsername(subject);
    return new UsernamePasswordAuthenticationToken(userDetails,"",null);

}

private String getEmail(String token) {
    String subject = Jwts.parser()
            .setSigningKey(secretKey)
            .parseClaimsJws(token)
            .getBody()
            .getSubject();
    return subject;
}

public String resolveToken(HttpServletRequest request){
    return request.getHeader("Authorization");
}

public boolean validateToken(String jwtToken) {
    try {
        Jws&#x3C;Claims> claims = Jwts.parser().setSigningKey(secretKey).parseClaimsJws(jwtToken);
        return !claims.getBody().getExpiration().before(new Date());
    } catch (RuntimeException e) {
        return false;
    }


<strong>    }
</strong>}
</code></pre>



보다시피 인터페이스를 구현하거나 클래스 확장이 아니기에 정해진 규격이 없다.&#x20;

그저 내 인증, 인가 기능에 있어서 토큰 관련한 로직들을 모아놓는 곳이라 생각하면 이해하기 편할거같다.

