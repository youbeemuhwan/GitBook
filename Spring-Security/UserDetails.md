# UserDetails, SecurityCofing

**UserDetails**

Spring Security 에서 사용자의 정보를 담는 인터페이스이다.

Spring Security 에서 사용자의 정보를 가져올때 보통 UserDetails 객체를 통해 가져오고 전달한다.

마치 Dto 역할 이라고 생각하면 이해하기 편하다.

이러한 Dto 역할을 하는 만큼 각 사용자 마다 필요한 사용자 정보가 다 다르므로 보통 추가할 부분은 추가해서 구현한 뒤 사용한다. ex. CustomUserDetails

```
@RequiredArgsConstructor
public class CustomUserDetails implements UserDetails {
    private final Member member;
    public final Member getMember() {
        return member;
    }
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return member.getPassword();
    }
    @Override
    public String getUsername() {
        return member.getEmail();
    }
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}

```

필자는 위에 처럼 Member (유저 도메인) 를 의존성 주입을 통해 가져와 거기서 각 유저정보를 빼내 넣어주는 방식으로 진행하였다.

하지만 그냥 멤버 변수를 선언해서 변수를 뿌려주는 방식도 가능하다.

다만 그럴경우 getter 메서드를 만들어줘야한다.

~~과연 어떤 방식으로 멤버변수에서 값을 잡아서 유저정보를 가져오는지 궁금하다...~~

~~(구글링, 인터페이스 쪽을 게속 파고들어가도 잘 모르겠다..)~~

```
@SuppressWarnings("serial")
public class CustomUserDetails implements UserDetails {
    
    private String ID;
    private String PASSWORD;
    private String AUTHORITY;
    private boolean ENABLED;
    private String NAME;
    
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        ArrayList<GrantedAuthority> auth = new ArrayList<GrantedAuthority>();
        auth.add(new SimpleGrantedAuthority(AUTHORITY));
        return auth;
    }
 
    @Override
    public String getPassword() {
        return PASSWORD;
    }
 
    @Override
    public String getUsername() {
        return ID;
    }
 
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }
 
    @Override
    public boolean isAccountNonLocked() {
        return true;
    }
 
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }
 
    @Override
    public boolean isEnabled() {
        return ENABLED;
    }
    
    public String getNAME() {
        return NAME;
    }
 
    public void setNAME(String name) {
        NAME = name;
    }
 
}

```

### **UserDetails 의 오버라이드 메서드 목록**

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## UserDetailsService

사용자 정보를 DB에 직접 접근해 가져오는 인터페이스 이다.

```
public interface UserDetailsService {
    UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}

```

파라미터를 가지고 findBy\~\~ 를 통해 유저 객체를 가져온뒤 Userdetails 에 담아서 반환 해주면 되고

CustomUserDetails 이 있다면 거기에 담아서 반환해주면 된다.

필자는 여기에 유저객체가 == null 이면 오류를 던지는 밸리데이션도 넣어줬다.

## SecurityConfig

```
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity
        .httpBasic().disable()
                .csrf().disable()
                .cors().and()
                .authorizeRequests()
                .antMatchers("/home", "/create",  "/createForm")
                .permitAll()
                .anyRequest().authenticated()
                .and()
                .formLogin()
                .defaultSuccessUrl("/home")
                .failureUrl("/loginForm")
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
                .and()
                .logout()
                .logoutRequestMatcher(new AntPathRequestMatcher("/member/logout"))
                .logoutSuccessUrl("/main")
                .invalidateHttpSession(true)
                .and()
                .build();
```

해당 코드는 필자가 토이프로젝트에 사용한 시큐리티 설정이다.

httpSecurity 를 반환값으로 놓고 빌더 형식으로 각 옵션 값을 주는 방식으로 설정한다.

csrf : 해당 옵션을 활성화하면 CSRF 토큰이 발급되는 필터를 시큐리티 가 자동으로 설정해준다.

하지만 비활성화 해놓은 이유는 REST API 로 개발했기 때문이다.

REST API 는 stateless 하게 개발하기에 유저 세션을 따로 저장하지 않는다.

또 JWT 토큰을 사용할때에도 CSRF 공격의 가능성이 낮아지므로 비활성화 시켜놓았다.

그리고 IDE 요청 시 IDE 에는 csrf 토큰이 없어서 403 에러도 발생하는것도 비활성화에 이유 중 하나이다.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

CSRF : Cross-site request forgery 의 약자로 사이트 간 요청 위조 이다. 웹사이트 취약점을 노린 공격으로 사용자가 자기도 모르게 공격자가 의도한 행위를 사이트에 요청을 하여 행하게 하는 공격이다.

주로 쿠키(Session ID) 를 탈취한 뒤 공격자가 의도한 행위, 즉 임의의 URL 로 Request 를 도용, 위조 하는것이다.

### 예시

1. 유저는 A 라는 사이트에 로그인을 했고 쿠키에 세션ID 값을 가진 상태이다.
2. 유저는 한 게시글을 클릭했고 해당 게시글은 공격자가 이미지 SRC에 유저의 비밀번호를 변경하는 URL을 담아놓았다.
3. 이미지는 페이지를 열때 자동으로 링크를 통해 오픈 되므로 해당 URL 이 실행되어 유저의 비밀번호가 공격자에 의해 임의로 변경되는것이다.

### 방지 방법

1. **CSRF 토큰을 발급해 임의의 요청을 진행할때 해당 토큰을 확인해서 정말 유저가 요청한것인지 한번 더 확인하는 방법**
2. **Referrer 검증**
3. **Double Submit Cookie 검증**

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

### SecurityConfig

cors() : cors 관련된 설정을 할수있다.

authorizeRequests() : 시큐리티 처리에 HttpServletRequest를 이용한다는 것을 의미합니다.

antMatchers() : 해당 설정에 경로를 적어주고 뒤에 .hasRole() , permitAll() 등을 붙혀서 경로 마다 접근설정을 할수있다.

.hasRole() : 괄호 안에 설정한 권한을 가진 사람만 해당 경로에 접근이 가능하다.

.permitAll : 해당 경로는 누구나 접근이 가능하다.

permitAll 을 해도 서큐리티 필터에 걸리면서 해당 URL 일 시 서큐리티 필터에서 request 에 임의 값을 넣어 필터체인을 통과되게 동작



.anyRequest().authenticated() : 어떤 요청이든 인증이 되야 한다.

.formlogin() : 스프링 시큐리티가 지원해주는 폼 로그인 방식을 사용한다.

.loginPage() : 괄호 안에 경로를 로그인페이지로 사용한다. 설정 하지 않을 시 스프링이 제공하는 기본페이지 호출

loginProcessingUrl() : 로그인을 처리하는 url 을 설정할수있다.

defaultSuccessUrl : 정상적으로 로그인이 성공했을때 이동하는 페이

failureUrl : 로그인 실패 시 이동하는 페이지

failureHandler : 로그인 실패 시 동작하는 핸들러를 설정할수있다.

successHandler : 로그인 성공 시 동작하는 핸들러를 설정할수있다.

```
http.addFilterBefore(new CustomAuthenticationProcessingFilter("/login-process"), 
                UsernamePasswordAuthenticationFilter.class);
```

addFilterBefore : 커스텀 필터를 뒤에 있는 필터보다 먼저 실행되게 설정

addFilterAfter : 커스텀 필터를 뒤에잇는 필터 뒤에 실행되게 설정

addFilterAt : 지정된 필터 순서에 커스텀 필터 실행 되게 설정
