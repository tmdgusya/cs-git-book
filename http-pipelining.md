# HTTP Pipelining

### 기존

HTTP/1.0 에서는 여러개의 Request 와 Response 를 순차적으로 받기 위해 아래와 같은 방식으로 Transaction 에서 요청과 응답을 처리했다.

![](<.gitbook/assets/image (2) (1).png>)

위와 같은 형테이므로 여러건의 요청이 오게됬을때 하나의 과정 (요청을 보내고 응답을 받기까지) 이 마치고 난뒤에야 다음 요청에 대한 과정을 진행할 수 있는 구조였다. 그래서 **여러건의 요청**을 보내야 하는 요청-응답의 구조가 순차적이므로 상황에서는 시간이 오래걸릴수 밖에 없었다.

또한, 한가지 문제가 더 있었는데 똑같은 도메인에 대해 요청건을 많이 보내게 될 경우가 대다수인 브라우저 환경에서 새로운 요청을 보낼때 다시 **TCP Connection** 을 맺게 되는 경우 TCP 의 Slow Start 나, 다시 3-Way-HandShake 를 하는 것에 대한 부담이 증가하게 된다. HTTP/1.1 에서는 위와 같은 성능향상을 위해 새로운 기능들을 도입했다.

### HTTP/1.1 의 Persistence Connection

**Persistence Connection(영속적인 연결)** 은 동일 도메인에 대한 요청이 대다수인 웹 브라우저 환경에서 같은 도메인의 대한 연속적인 요청들이 새롭게 **TCP Connection 을 만드는 과정의 오버헤드**를 줄이기 위해 등장했다. (**TCP 는 그리고 기본적으로 Flow Controll 을 하기 때문에 이미 사용된 TCP Connection 를 사용하는 것이 좋다**.) 이는 HTTP/1.1 이전 에도 **Connection Header 에 keep-alive** 설정을 통해 가능하다. 하지만 Dumb Proxy 등의 문제가 발생할 수 있는 여지가 있다. 하지만 **HTTP/1.1 의 경우에는 기본적으로 keep-alive 설정과 같이 지속적인 연결을 기반으로 TCP Connection 이 설립**된다. 단기 커넥션을 이용하고 싶다면 `Connection: close` 헤더를 이용하면 된다.

<img src=".gitbook/assets/image (3).png" alt="" data-size="original">

### HTTP/1.1 의 Pipelining

Pipelining 은 HTTP/1.1 이전버전 에서 3개의 요청을 보낼때 **(요청-응답) -> (요청-응답) -> (요청-응답) 의 순차적으로 진행되어 느린 부분**을 성능적으로 개선하기 위해 등장한 방법이다. 다만 HTTP/1.1 의 Pipelining 은 구현의 어려움과 대다수의 프록시서버에서 구현을 안해주는 문제로 대부분의 브라우저에서 기본적으로 활성화되지 않는것으로 알고 있다.&#x20;

일단 HTTP/1.1 의 Pipeline 아키텍쳐는 아래와 같다.

<img src=".gitbook/assets/image (2).png" alt="" data-size="original">&#x20;

Pipelining 을 이용한 방법또한 첫번째로 요청한 요청값에 대한 응답값을 먼져 받도록 설계되어 있으나, **기존처럼 하나의 Request 가 처리되는 동안 다른 Request 를 보내지 못했던 방법보다 Network Latency 가 덜하다**. MDN 문서를 보니 **두개의 요청을 하나의 패킷으로 Packing** 해서 성능적인 향상 또한 있다고 한다. 다만 모든 종류의 HTTP Method 가 Pipelining 방법으로 처리될 수 있는 것은 아니며, **멱등성(idempotent) 를 만족하는 HTTP Method 들에 한해서 Pipelining 이 가능**하다.

### Ref

{% embed url="https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x" %}
