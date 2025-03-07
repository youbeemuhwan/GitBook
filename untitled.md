# Untitled

## WEBMVC-LIB

해당 모듈을 이해하려면 해당 모듈의 기능들을 도와주는 공통 기능에 대해 알아야한다.

#### HandlerMethodArgumentResolver

> 인터페이스는 어떤 객체, 클래스에 대한 기능을 구현하는데 목적이 있다.
>
> 필드값(상태), 생성자를 가질 수 없기에 객체 자체를 표현하는데는 어려움이 있다.
>
> 다만 다중 상속이 가능하기에 객체가 어떤 기능을 할것인가 를 정의하는 용도로 쓰인다.

컨트롤러가 Request 에서 받은 파라미터를 처리할때 도움을 주는 인터페이스 이다.

해당 인터페이스에는 대표적인 메서드가 두개 있다.

* supportsParameter()

```
@Override public boolean supportsParameter(MethodParameter parameter) { 
// @LoginUser 어노테이션이 붙어 있고, User 타입이면 지원 return 

parameter.hasParameterAnnotation(LoginUser.class) && parameter.getParameterType().equals(User.class); }
```

* resolveArgument()

