# Ajax 란?

## Ajax

JavaScript 은XML 을 통해 클라이언트와 서버의 비동기 통신을 지원하는 기술이다.



비동기 통신이기 때문에 서버의 처리가 완료될때까지 매번 기다리지 않아도 처리가 가능하다.

웹페이지에는 많은 파트들 이 있는데 한 파트의 데이터만 변경하거나 불러오는 등 서버 통신이 필요하다면 전체를 새로고침 하는것이 아닌 그 파트만 서버와 통신할수있기에 비용 시간에 있어 효율적이다.



주로 JQuery 와 같이 사용한다. JQuery 와 같이 사용하면 적은 코드량, HTTP 메소드 선택 등 더 쉽게 처리가 가능하다.

```
  $(document).ready(function() {
      $.ajax({
        type : "HTTP 메서드 형식 지정",
        data : "해당 서비스에 접근할때 병원 측에서 요구하는 데이터 (파라미터) 등을 담는 곳" 
        url : "해당 서비스(리소스) 에 접근하기 위한 서버 url",
        dataType : "반환 값의 타입을 지정 ex. xml, json",
        contentType : "application/x-www-form-urlencoded;charset=UTF-8"
        error: function() {
          console.log('통신실패!!');
        },
        success: function(data) {
          console.log("통신데이터 값 : " + data);
```

url : 해당 서비스(리소스) 에 접근하기 위한 서버 url

data : 해당 서비스에 접근할때 병원 측에서 요구하는 데이터 (파라미터) 등을 담는 곳 (주로 변하지 않는 일정한 값을 보내는거 같다.)

type : HTTP 메서드 형식 지정

dataType : 반환 값의 타입을 지정 ex. xml, json

timeout : 해당 시간 동안 요청에 미응답 시 타임아웃

Success : 성공 시 로직

error : 에러 시 로직





