# CORS(Cross-Origin Resource Sharing)
최근 웹 개발 환경은 Client 와 Server를 분리하고 Rest API 형태로 데이터를 요청하게 된다. <br/>
여기서 사용자의 Client가 화면에 띄우기 위해 필요한 데이터를 Server에게 요청하게 되는데 여기서 CORS 정책을 처리하지 않으면 데이터를 주고 받을 수 없다.
우리 말로 _교차 출처 자원 공유_ 라고 하며 서로 다른 출처(Origin)에서 자원을 가져올 때 안전하다는 보장을 하기 위한 정책이다.
<br/>
<br/>


## Origin 이란?
흔히 우리가 사용하는 URL은 'http://localhost:8080/user?name=jo 와 같은 형태로 사용된다. <br/>
여기서 우리는 Protocol + Host + Port 가 같으면 동일한 Origin 이라고 한다.

<img src="https://user-images.githubusercontent.com/29935137/224641241-65472ea8-2ff9-4f3f-975b-8bdbfb9433ad.png" width="1000px" height="200px">

보통 REST API로 설계하게 된다면, 같은 프로토콜에서 동작하여도, 사용자 컴퓨터와 Server 컴퓨터 간의 Host가 다르고 Port 또한 다르게 열릴 수 있기 때문에 이에 대한 처리를 하게 된다.
<br/>
<br/>

## CORS의 구현 위치
이러한 CORS는 Server나 Client에서 구현된 로직이 아니라 브라우저 자체에 구현 스펙이다. 따라서 우리가 Client에서 Server로 요청을 보낸다면 Server는 정상적인 응답을 보낼 것이다. <br/><br/>
단, 브라우저 자체에서 해당 응답은 다른 출처이므로 무시하게 된다. 따라서 이 문제를 해결하기 위해서 Server 단을 찾아다니면 문제 해결에 어려울 수도 있다.
<br/>
<br/>

## CORS의 동작 방식
우선 웹 클라이언트가 다른 출처에 요청을 보낼 때 HTTP로 보내게 된다. 여기서 Header에 Origin에다 본인의 출처를 작성하여 보내게 된다.

```http
Origin: https://localhost:3000
```

Server는 요청에 대한 응답으로 Header의 Access-Control-Allow-Origin이라는 값에 “이 리소스를 접근하는 것이 허용된 출처”를 내려주고, 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 Origin과 서버가 보내준 응답의 Access-Control-Allow-Origin을 비교해본 후 이 응답이 유효한 응답인지 아닌지를 결정한다.

CORS를 하기 위한 방식으로 Preflight Reques, Simple Request, Credentialed Request 가 존재한다.
<br/>
<br/>

### Preflight Request
이는 원하는 요청을 보내기 전에 작은 크기의 예비 요청을 미리 보내서 안전한 환경인지 확인하는 방식이다. 

이 때 예비 요청에는 본 요청에 대한 정보를 미리 알려준다

```ht
Access-Control-Request-Headers: content-type     // 본 요청에 쓰일 content-type
Access-Control-Request-Method: GET               // 본 요청의 method
```

여기서 중요한 것은 예비 요청의 성공 여부 보다 응답 헤더에 `Access-Control-Allow-Origin`에 알맞은 값이 있느냐 이다.

### Simple Request
위의 방식이 예비 요청을 보내는 것이라면, 이 방법은 특정한 조건을 만족하면 그냥 바로 본 요청을 보내는 것이다.<br/>

특정한 조건은 다음과 같다.<br/>
1. 요청의 메소드는 GET, HEAD, POST 중 하나여야 한다.
2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안된다.
3. 만약 Content-Type를 사용하는 경우에는 application/x-www-form-urlencoded, multipart/form-data, text/plain만 허용된다.

이는 보통의 웹 서비스에서 모두 만족하기 어려워 사용하기 어렵다고 한다.

### Credentialed Request
보안을 강화하고 싶을 때 사용하는 방식으로 인증된 요청을 사용하는 방법이다.

PASS

## CORS 해결법
### Access-Control-Allow-Origin
가장 보편적인 방법으로 Server에서 Access-Control-Allow-Origin을 설정하여 처리하는 방법이다. <br/>
게으르고 귀찮은 개발자라면 단순히 `*`을 사용하여 모든 요청에 대한 처리를 하겠지만, 우리 착한 친구들은 `Acess-Control-Allow-Origin: https://github.com` 처럼 출처를 지정해줍시다.

### Webpack Dev Server
FE 개발자의 경우 Webpack 을 사용하여 개발 환경을 구축할 때 리버스 프록시 방법을 사용하여 CORS를 우회할 수 있다.

```Javascript
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'https://api.google.com',
        changeOrigin: true,
        pathRewrite: { '^/api': '' },
      },
    }
  }
}
```

이런식으로 요청을 보내게 되면 브라우저는 자신의 출처에 https://localhost:8080/api와 같이 요청을 보낸 것으로 오해하여 CORS를 우회할 수 있다.

# Reference
https://evan-moon.github.io/2020/05/21/about-cors/#preflight-request
