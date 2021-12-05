# ConfigurableApplicationContext

## 배경

Socket과 관련된 메소드들을 가진 SocketManager 클래스

처음 Spring boot 시작 직후 Socket도 열어주기 위해 main 메소드에서 생성자로 직접 객체 생성 후 함수 실행

이후 해당 클래스에 있는 메소드를 다른 서비스 클래스 내의 메소드에서 실행 시도 시 문제 발생

## 내용

### 호출하는 부분
```java
@RequiredArgsConstructor
@Service
public class AttendanceService {

    private final SocketManager socketManager;

    ...

    public ResponseDto requestAttendanceCheck(Long lectureId, Long lessonId, String session) {

        ...

        socketManager.sendAttendanceRequestEvent(session);

        return new ResponseDto("SUCCESS");
    }
}

```
### 호출되는 부분
```java
@Component
public class SocketManager implements ApplicationListener<ContextClosedEvent> {

    ...

    public void sendAttendanceRequestEvent(String room) {
        for (SocketIOClient roomClient : mainNamespace.getRoomClients(room)) {
            if(roomClient.getSessionId().equals(roomHosts.get(room))) {
                continue;
            }
            roomClient.sendEvent("attendance request", "response for professor's attendance check request");
        }
    }
}
```

### 1번 코드
```java
@EnableJpaAuditing
@SpringBootApplication
public class OneboardServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(OneboardServerApplication.class, args);

		SocketManager launcher = new SocketManager();
		launcher.startSocketIOServer();
	}
}
```
`requestAttendanceCheck()` 메소드에서 `sendAttendanceRequestEvent()` 메소드 호출은 되나 `mainNamespace.getRoomClients()` 의 결과가 없음<br>
같은 `SocketManager` 클래스의 다른 메소드에서 `sendAttendanceRequestEvent()` 메소드 호출 시에는 정상 작동

`SocketManager` 클래스를 생성자로 직접 객체 생성해서 Spring Bean 관리상 문제가 있는 것으로 의심

2번 코드 시도

<br>

### 2번 코드
```java
@EnableJpaAuditing
@SpringBootApplication
public class OneboardServerApplication {

	@Autowired
	private SocketManager socketManager;

	public static void main(String[] args) {
		SpringApplication.run(OneboardServerApplication.class, args);

		socketManager.startSocketIOServer();
	}
}
```
`@Autowired`를 통해 의존성 주입 후 사용 시도<br>
`socketManager.startSocketIOServer();` 에서 NullPointException 발생

3번 코드 시도

<br>

### 3번 코드
```java
@EnableJpaAuditing
@SpringBootApplication
public class OneboardServerApplication {

	@Autowired
	private SocketManager socketManager;

	public static void main(String[] args) {
		ConfigurableApplicationContext context = SpringApplication.run(OneboardServerApplication.class, args);

		OneboardServerApplication main = context.getBean(OneboardServerApplication.class);
		main.socketManager.startSocketIOServer();
	}
}
```
context 객체를 통해 main 메소드가 포함된 클래스의 빈 객체를 받아와 실행<br>
정상 작동

`ConfigurableApplicationContext` 및 빈 관리에 대한 추가 학습 필요

## Reference
- [[Spring Boot] 한정자](https://araikuma.tistory.com/53)