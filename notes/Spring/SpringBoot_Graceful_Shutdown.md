# Spring Boot 종료 시 메소드 수행 (Graceful Shutdown)

## 배경

학교 캡스톤 프로젝트인 OneBoard 프로젝트의 백엔드 작업 중 비대면 수업에 참여한 강의자, 학생들 간의 출석, 퀴즈, 이해도 평가 요청/응답을  Socket.io 를 사용해 구현하기로 하였다.

그런데 해당 Socket 작업 중 Spring Boot 애플리케이션 종료 시, Socket 통신에 사용되는 port 가 제대로 반환되지 않아 계속 물고 있게 되어 바로 재시작 했을 때, Socket 서버 시작에 실패하는 것을 확인하였다.

따라서 이를 해결하기 위해 Spring Boot 애플리케이션 종료 시 Socket 중지 메소드를 실행하여 Socket 서버가 제대로 종료될 수 있도록 하고자 한다.

## 작업

```java
public class SocketManager implements ApplicationListener<ContextClosedEvent> {
    
    ...
    
    @Override
    public void onApplicationEvent(ContextClosedEvent event) {
        System.out.println("애플리케이션이 종료되어 소켓 연결을 중지합니다");
        stopSocketIOServer();
    }
}
```
`ApplicationListener<ContextClosedEvent>` 인터페이스를 구현하여 `onApplicationEvent` 메소드 안에 Socket 서버를 중지하는 메소드를 넣어주어 Spring Boot 애플리케이션 종료 시 Socket 서버 또한 정상 종료될 수 있도록 했다.

추가로 `onApplicationEvent` 는 애플리케이션 종료 이벤트 발생 시 단 1번 실행되며 정상 종료 시에만 실행된다.
- 정상 종료 = SIGTERM = kill -15
- 비정상 종료 = SIGKILL = kill -9

## Reference
- [스프링 애플리케이션이 시작, 종료될 때 수행할 메서드 지정하는 방법 + 스프링 빈(Bean)이 생성, 소멸될 때 수행할 메서드 지정하는 방법(graceful 종료, CommandLineRunner, ApplicationListener, InitializingBea..](https://jeong-pro.tistory.com/179)
- [Spring Boot 실행과 종료 시 특정 동작을 실행하도록 해보기](https://zepinos.tistory.com/41)
- [[SpringBoot] destroy event 등록하기](https://bamdule.tistory.com/229)
- [Spring boot Graceful Shutdown](https://bravenamme.github.io/2020/10/06/graceful-shutdown/)
- [Spring Boot - 안전하게 종료시키기](https://heowc.dev/2018/12/27/spring-boot-graceful-shutdown/)