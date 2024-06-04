# Page 1

### DESCRIPTION

작성된 서식 데이터 불러오기



Token 안에 있는 유저정보를 통해 작성된 서식이 있다면 불러온다.



URL

https://ums.ish.or.kr/get"



### HTTP METHOD

POST



### REQUEST :

```
{ 
    token : {token Data (String)} 
}
```

### RESPONSE :

```
{ 
    form Data (String)
}
```





### DESCRIPTION





Token 안에 있는 유저정보를 통해 작성한 서식을 저장한다.

### URL

https://ums.ish.or.kr/saveData



### HTTP METHOD

POST



### REQUEST :

```
{ 
    token : { String },
    FORM_DATA : { String }
}
```

### RESPONSE :

```
VOID
```





### DESCRIPTION

BYPASS 페이지 진입



BYPASS에 진입 할 시 토큰이 존재 할 경우 토큰 유효성을 검증하여 토큰 정보를 통해 해당 서식 페이지로 이동한다.



토큰이 없다면 URL 의 patientKey 를 가지고 환자 존재 여부 판단 후 \
해당 정보로 토큰을 생성, 환자의 서식 정보를 가져와 해당 서식 으로 이동한다.



TEST 환자로 BYPASS 진입 시 기존 TEST 환자가 가지고 있는 서식값으로만 진입 가능하다.

### URL

https://ums.ish.or.kr/untact/bypass/{patientKey}?preview=



### HTTP METHOD

POST



### PARAMETER

```
patientKey : String

preview : Y or N,  default = N 
```

### REQUEST :

```
{ 
    passid : BUNHO AT VW_ARUM_UNTACT_OUT_LIST,
    passkey : MEMBER_SEQ AT NFTF_QUESTIONNAIRE_VIEW
}
```

### RESPONSE :

```
Preview = Y -> bypass-previw 페이지 호출

else -> bypass 페이지 호출
```





### DESCRIPTION

테스트 페이지 진입



환자는  각각 자신이 작성해야될 서식 컬럼을 가지고 있다.

그래서 기존 로직은 해당 서식값을 가져와 그에 맞는 서식지를 불러오는 방식이지만

테스트의 경우 한 환자를 가지고 여러 서식을 작성해야된다는 요구사항에 맞게

URL 의 {urlName} 파라미터를 가지고 해당 서식페이지로 이동하게끔 하드코딩 되있다.



### URL

https://ums.ish.or.kr/untact/test/{userInfo}/{urlName}

### HTTP METHOD

GET



### PARAMETER

```
userInfo : IO_GUBUN+BUNHO (ex. I232112)
urlName: {
        
"endocrine"
"urology"
"plastic"
"brth"
"dermatology"
"obgyn"
"main"
"thyroid"
}


```



### RESPONSE :

```
testStartPage 페이지 호출
```





### DESCRIPTION

본인인증 페이지 진입



### URL

https://ums.ish.or.kr/untact/{userInfo}

### HTTP METHOD

GET



### PARAMETER

```
userInfo : IO_GUBUN+BUNHO (ex. I232112)
```

### RESPONSE :

```
checkplus_main 페이지 호출 (본인인증 페이지)
```





### DESCRIPTION

바이패스용 토큰 생성 로직

* 기존 토큰은 본인인증 완료 시에 NICE 에서 제공받는 데이터를 바탕으로 토큰을 생성한다.\
  \
  반면  바이패스의 경우 본인인증을 스킵하기에 전달받는 JSON 데이터를 바탕으로 토큰을 생성한다. &#x20;



### URL

https://ums.ish.or.kr/untact/bypass/createToken/{userInfo}

### HTTP METHOD

POST



### PARAMETER

```
userInfo: IO_GUBUN+BUNHO (ex. I232112)
```



### RESPONSE :

```
BypassTokenDto : 
{
    token : String,
    FORM_GUBUN : 서식 이름 ex. endocrine, obgyn
}
```





### DESCRIPTION

환자정보를 바탕으로 검증 후 토큰 생성한다.

* 테스트 페이지 진입 시 하드코딩된 테스트 유저 데이터를 바탕으로 토큰을 생성한다.

### URL

https://ums.ish.or.kr/untact/checkPatientInfo/test

### HTTP METHOD

POST

### REQUEST :

```
{ 
   certifiedName : "유저이름" :  String
   certifiedBirth : "유저 생년월일" (YYYYMMDD) : String
   userInfoCode : IO_GUBUN+BUNHO : String
   formName : "서식 명"  : String  EX. endocrine, obgyn
}
```

### RESPONSE :

```
TokenDto : {
     token : String    
}
```



### DESCRIPTION

&#x20; 본인인증 완료 시 본인인증 정보와 URL 환자정보를 대조해 검증하고 토큰 발급



### URL

https://ums.ish.or.kr/untact/checkPatientInfo

### HTTP METHOD

POST

### REQUEST :

<pre><code>{ 
<strong>    certifiedName : "유저이름" :  String,
</strong>   certifiedBirth : "유저 생년월일" (YYYYMMDD) : String,
   userInfoCode : IO_GUBUN+BUNHO : String,
}
</code></pre>

### RESPONSE :

```
TokenDto : {
     token : String    
}
```





### DESCRIPTION

서식 페이지 진입 시 토큰 검증

### URL

https://ums.ish.or.kr/untact/validateToken/ByFormName

### HTTP METHOD

POST

### REQUEST :

<pre><code>{ 
<strong>    token : String,
</strong><strong>    formName : String  Ex. endocrine, obgyn
</strong>}
</code></pre>

### RESPONSE :

```
resultDto : {
     certifiedBirth : "유저 생년월일" (YYYYMMDD) : String
     PATIENT_KEY : "환자키" ( IO_GUBUN + NAEWON_KEY) : String
     PATIENT_CODE : "BUNHO" : String
     FORM_NAME : "서식 명" : String EX. endocrine, obgyn
     
}
```



### DESCRIPTION

본인인증 페이지 진입 시 토큰이 존재한다면 토큰 검증증

### URL

https://ums.ish.or.kr/untact/validateToken/ByPatientKey

### HTTP METHOD

POST

### REQUEST :

<pre><code>{ 
<strong>    token : String,
</strong><strong>    patientKey: "환자키" ( IO_GUBUN + NAEWON_KEY) : String
</strong></code></pre>

### RESPONSE :

```
resultDto : {
     certifiedBirth : "유저 생년월일" (YYYYMMDD) : String
     PATIENT_KEY : "환자키" ( IO_GUBUN + NAEWON_KEY) : String
     PATIENT_CODE : "BUNHO" : String
     FORM_NAME : "서식 명" : String EX. endocrine, obgyn
     
}
```

