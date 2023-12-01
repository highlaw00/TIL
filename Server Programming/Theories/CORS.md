# CORS
### 시나리오
CORS가 무엇인지, 어떻게 조작하는지 알아보기 전에 왜 필요한지부터 이해해보자.
다음과 같은 일이 벌어졌다고 가정해보자.
> 평소와 같이 웹 서핑 중인 최율 씨. 의도치 않게 `http://badsite.com`에 접속했더니, 해당 악성 사이트에서 최율 씨의 브라우저에 자바스크립트 파일을 실행시켜 `http://들어가면큰일나는사이트.com`에 API 요청을 했다. 그 결과로 최율 씨는 엄청난 일을 당하게 된다... (원치 않던 자산 매도, 개인 정보 유출 등...)

위와 같은 일이 왜 발생했을까? `http://badsite.com`에 접속하였을 때, 브라우저가 자바스크립트 파일을 실행하였을 것이고 해당 자바스크립트 파일 내에는 `http://들어가면큰일나는사이트.com`에 API 요청을 하는 코드가 존재했기 때문이다. 만약 이런식의 피싱 방법이 가능하다면 웹 서비스를 사용하는 입장에서 상당한 피해가 발생할 것이다. 따라서 이런 보안상의 이슈를 막고자 존재하는 것이 **SOP, Same-Origin-Policy**이다
### SOP

SOP는 위와 같은 시나리오가 발생하는 것을 방지한다. 브라우저는 동일한 도메인간의 API 호출이 아닌 경우 API 호출이 실행되었을 때, 실행되지 않도록 오류를 발생시킨다.

그렇다면 SOP가 언제나 옳을까? SOP는 타 도메인간 데이터 송수신을 막아 보안상의 이점을 가지지만 다양한 웹 서비스가 서비스끼리의 데이터를 송수신하는 경우가 많다. 예를 들자면, 어떤 웹 사이트에 회원가입을 진행 할 때, `카카오 계정으로 로그인하기`와 같은 버튼을 본 적이 있을 것이다. 이 경우 해당 버튼을 누르면 내가 사용하려는 웹 사이트와 카카오 서버 간의 데이터 전달이 진행되어야 한다. 그러나! SOP가 이것을 막는다면 다양한 웹 서비스를 구축하기 어려울 것이다.

그러한 이유로 등장한 것이 **CORS**이다.

## CORS
>CORS: Cross-Origin Resource Sharing

이름에서 엿볼 수 있듯, `Origin`이 다른 것들의 자원 공유를 허가해준다는 의미이다. 그렇다면 공유를 막는 것은 사용자의 브라우저인데 이것을 어떻게 허가해줄 수 있을까? 바로 서버에서 이것을 허가해주어야 한다.
HTTP 요청과 응답 메시지를 보며 이해해보자.

### 예시
>GET /resources/public-data/ HTTP/1.1
Host: **bar.other**
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: **https://foo.example**

위 메시지는 브라우저가 서버로 전송한 요청 메시지이다. 보다싶이 `https://foo.example`에서 HTTP 요청을 보냈음을 확인할 수 있다. 서버의 도메인은 `bar.other`다. 요청에 대한 응답 메시지는 다음과 같다.

> HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 00:23:53 GMT
Server: Apache/2
Access-Control-Allow-Origin: *
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/xml

`Access-Control-Allow-Origin`의 값이 `*`인 응답 메시지를 보냈다. 이것의 의미는 `bar.other` 서버가 모든 도메인에 대해서 자신의 데이터를 넘겨주겠다는 의미이다.

브라우저는 요청 메시지의 `Origin`과 응답 메시지의 `Access-Control-Allow-Origin`을 대조하여 허가되지 않은 도메인이라면 요청을 차단한다!