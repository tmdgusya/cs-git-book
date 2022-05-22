# End-To-End Header / Hob-by-Hob Header

### End-To-End Header

* end-to-end Header 는 마지막 Receiver 에게 전달되어야 하는 Header 입니다. 따라서 Proxy 서버에서 이를 임의로 변경해서는 안됩니다.
* Cache-Entry 의 일부로 저장되어야&#x20;

### Hob-By-Hob Header

* 현재 Transaction 에서 사용되는 Header 입니다.

### Difference

* 쉽게 설명하면 우리의 브라우저(클라이언트) -> Web 서버 Container(Nginx) -> Docker (Application Container, Final Receiver) 라고 했을때 End-To-End Header 는 Docker 또는 Client 에서 사용되는 Header 이고, Hob-By-Hob Header 는 Client <-> Nginx 사이에서 사용되는 Header 라고 생각하면 쉬운 것 같다.

### Ref

* [https://developpaper.com/hop-by-hop-headers-vs-end-to-end-headers/](https://developpaper.com/hop-by-hop-headers-vs-end-to-end-headers/)
* [https://datatracker.ietf.org/doc/html/rfc2068](https://datatracker.ietf.org/doc/html/rfc2068)&#x20;
