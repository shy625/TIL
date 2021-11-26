# API vs SDK

## API
- Application Programming Interface

### API의 특징
- Communication
    - 앱과 앱, 서비스와 서비스가 통신하는 방법
- Abstraction
    - 다른 서비스 내부에서 어떻게 동작하는지 요청하는 서비스에서 알 필요가 없다
- Standard
    - 표준 규약이 있다
    - SOAP, GraphQL, REST

### API의 구성요소 - REST API
- REpresentational State Transfer

#### **Request**
- operation
    - HTTP Method - GET, POST, PUT, DELETE ...
- parameter
    - Optional
- endpoint
    - 요청을 보내고자 하는 상대 서비스의 요청 url

#### **Response**
- 보통 JSON 과 같은 Raw 타입 데이터

## SDK
- Software Development Kit
- API를 호출하는 코드로 이루어져 있음
- 언어별 SDK가 존재
- 사용자는 직접 API를 구성하고 호출할 필요없이 해당 API를 호출하는 SDK의 함수를 가져다 사용하면 됨

### Reference
- [API vs. SDK : 차이점은 무엇입니까?](https://www.youtube.com/watch?v=kG-fLp9BTRo&list=LL&index=1)