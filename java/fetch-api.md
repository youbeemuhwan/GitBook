---
coverY: 0
---

# Fetch API

JS 에선 비동기 통신을 하여 데이터를 받아오거나 데이터를 보낼수있다.

이번 프로젝트에서 프론트단에서 비동기통신을 해야하는 일이 생겼고

이러한 비동기 통신을 하는 방법은 다양했다. 그 중에 Fetch API 에 대해 작성해보려 한다.



### Fetch API  선택 이유

&#x20;Ajax, Axios 같은 선택지도 있었지만 일단 지금 프로젝트는 제이쿼리 사용 없이 순수 JS 로만 진행하기 때문에 자연스레 Ajax 는 밀려났다. 또한 Ajax, Axios 둘 다 따로 모듈을 설치해야 한다는 점 에서도.&#x20;

Fetch API 는 내장형이기 때문에 따로 설치가 필요없으니 그런 면에서는 업데이트 관련 안정성이 더 높다고 생각했다.  하지만 Fetch API  는Ajax, Axios 에 비해 기능이 적고 리턴값에 대해 JSON 으로 직접 변환해줘야 하며 IE11 을 지원하지 않는 점도 고려할 부분이였다.

&#x20;하지만 프로젝트의 로직 자체가 복잡하지 않고 단순 데이터 통신이며  프로젝트 사용 대상이 IE 사용이 극히 적기에 가볍고 단순한것을 기준으로 잡았다. 그런 기준에는  Fetch API 가 가장 기준에 부합했다.&#x20;



### Fetch API&#x20;



#### POST

```
// POST 메서드 구현 예제
async function postData(url = "", data = {}) {
  // 옵션 기본 값은 *로 강조
  const response = await fetch(url, {
    method: "POST", // *GET, POST, PUT, DELETE 등
    mode: "cors", // no-cors, *cors, same-origin
    cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
    credentials: "same-origin", // include, *same-origin, omit
    headers: {
      "Content-Type": "application/json",
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    redirect: "follow", // manual, *follow, error
    referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
    body: JSON.stringify(data), // body의 데이터 유형은 반드시 "Content-Type" 헤더와 일치해야 함
  });
  return response.json(); // JSON 응답을 네이티브 JavaScript 객체로 파싱
}

postData("https://example.com/answer", { answer: 42 }).then((data) => {
  console.log(data); // JSON 데이터가 `data.json()` 호출에 의해 파싱됨
});
```

다양한 옵션들이 있고 기본적인 틀을 보면 Ajax 와 굉장히 유사하다는 느낌을 받았다.

특이사항은 없고 body 부분을 보면  data 를 따로 JSON 으로 직접 변환시켜줘야 한다.



#### GET

```
//GET 메서드 구현 예제
function request() {
  fetch('https://api.github.com/orgs/nodejs', {
    method: 'GET',
  })
  .then(response => {
    return response.json();
  })
  .then(data => {
    console.log(data);
  });
}
request();
```

method 기본값이 GET 이므로 생략해도 무방하다.&#x20;



사용해보니 에러처리도 지원해주고 Promise 형태라  결과값을 가공하는데 있어서도 어려움이 없었다.

복잡하고 특정한 로직을 요구하는게 아니라면 추후에도 FETCH API 를 사용하는것을 고려해봐야겠다는&#x20;

생각이 드는 경험이었다.

