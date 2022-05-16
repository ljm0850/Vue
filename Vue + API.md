# Vue + API

## Server & Client

- Server (정보 제공)
  - 클라이언트에게 **정보**,**서비스** 제공하는 시스템
  - Django를 통해 template 제공
  - DRF를 통해 JSON

- Client (정보 요청 & 표현)
  - 서버에게 서비스 요청
  - 서비스 요청시 필요한 인자를 서버 요구 형식에 맞게 제공
  - 서버에게 받은 응답을 사용자에게 표현



## CORS

### Same-origin policy (SOP)

- 동일 출처 정책
- 특정 출처(origin)에서 불러온 문서나 스크립트를 다른 출처에서 가져온 리소스와 상호 작용 하는것을 제한
  - 보안

#### Origin(출처)

- URL의 **Protocol**,**Port**,**Host**가 모두 같아야 동일한 출처
  - Protocol- http https
  - Port- http://테스트.테스트url.com**:80**/Path (http 기본값이라 주로 생략)
  - Host-http://**호스트**.com/Path



### Cross-Origin Resource Sharing (CORS)

- 추가적인 **HTTP header**를 사용하여 특정 출처에서 실행중인 웹이 다른 출처의 자원에 접근 할 수 있는 권한을 부여하도록 **브라우저**에 알려줌
  - 권한이 없을 경우 서버는 정상적으로 응담하지만, 브라우저에서 차단
- 올바른 CORS header를 포함한 응답을 반환 하여야 가능

- CORS는 HTTP의 일부, 어떤 호스트에서 자신의 컨텐츠를 불러갈 수 있는지 서버에 지정

- CORS HTTP 응답 헤더
  - Access-Control-Allow-Origin
  - Access-Control-Allow-Credentials
  - Access-Control-Allow-Headers
  - Access-Control-Allow-Methods



#### Access-Control-Allow-Origin

- `Access-Control-Allow-Origin: *` , `Access-Control-Allow-Origin:localhost:8080`
  - `*`: all
  - `:` 뒤에 주소 입력

- 응답이 주어진 출처(origin)로 부터 요청 코드와 공유 될 수 있는지를 나타냄

----