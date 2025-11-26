엔진엑스 컨테이너를 어떤 이유로 사용하는가?

1. 로컬에서 개발하기 위해서 리버스 프록시 용도로
로컬에서 FE, BE 개발하려면 nginx 필요함.

WS -> http://dev-community-ws:3000
WAS -> http://dev-community-was:8080 

엔진엑스에 접속할 때 http 통신만 가능

ssl 적용 안됨!!

2. 클라우드 서버에 배포 하는데 리버스 프록시 용도로 사용하기 위해서
   
그러면 public vpc에 위치해야 하고 인터넷 게이트웨이 그리고 고정아이피 할당 필요

다음 vpc 내부 네트워크를 통해서 WS와 WAS에 요청이 나뉘도록 지정해줘야 함

dev 환경에 배포
dns 레코드 설정 dev.idontwannabeyouanymore.com -> 할당받은 고정 아이피

인스턴스 하나에 Nginx, WS, WAS 컨테이너가 전부 돌아갈 때
WS -> http://dev-community-ws:3000
WAS -> http://dev-community-was:8080 

각각 인스턴스에서 Nginx, WS, WAS 컨테이너가 돌아갈 때
Nginx는 pulbic vpc에 있어야함.
WS -> 할당받은 http://private vpc ip:3000 ex) http://10.0.1.x:3000
WAS -> 할당받은 http://private vpc ip:8080 ex) http://10.0.1.x:8080

prod 환경에 배포
dns 레코드 설정 idontwannabeyouanymore.com -> 할당 받은 고정 아이피
Nginx는 pulbic vpc에 있어야함.
WS -> 할당받은 http://private vpc ip:3000 ex) http://10.0.0.x:3000
WAS -> 할당받은 http://private vpc ip:8080 ex) http://10.0.0.x:8080

엔진엑스에 접속 할 때, http, https 통신 둘 다 가능

ssl 인증서 관리 위해 cert 컨테이너 작동 필요함.
인스턴스 하나에 Nginx, cert, WS, WAS 컨테이너가 전부 돌아갈 때

각각 인스턴스에서 Nginx, cert, WS, WAS 컨테이너가 돌아갈 때
인증서는 s3에서 끌어오는 방식이 좋을듯.

3. 어떤 환경이든지 로드밸런서 역할을 하기위해서
아직 고민 공부 안해 봄.