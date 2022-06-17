# Scheduler

일정한 시각이나 일정한 시간 간격으로 특정 작업을 반복적으로 수행해야 하는 경우, 스케줄러를 사용하여 작업을 등록하고, 반복 주기를 설정함으로써 자동화할 수 있다.

각 OS마다 이러한 기능을 제공하는 스케줄러 프로그램이 있으며 윈도우는 작업 스케줄러, 유닉스 계열 OS는 Cron을 제공한다.

서버에는 일반적으로 리눅스가 많이 사용되고, 또한 CLI 환경으로 접근해서 사용하기 때문에 그러한 환경에서 사용되는 Cron에 대해서 알아보았다.

## Cron

**Cron**

- 유닉스 계열 OS의 시간 기반 잡 스케줄러

**Crontab**

- Cron 작업들에 대한 설정 파일 (cron table)

Cron 동작 시 crond(cron 데몬)은 crontab 파일을 참조하여 어떤 작업들을 어떤 주기로 실행해야 하는지 확인 후 실행한다.

OS 사용자마다 crontab 파일을 가질 수 있다.

### Cron 명령어

- 작업 생성 및 수정

    ```zsh
    crontab -e
    ```

- 작업 리스트 확인

    ```zsh
    crontab -l
    ```

- 작업 삭제

    ```zsh
    crontab -r
    ```

### 작업 주기 설정

#### 기본 형식

```zsh
# 형식
[분] [시간] [일] [월] [요일] "실행할 명령어"

# [분]: 0 - 59
# [시간]: 0 - 23
# [일]: 1 - 31
# [월]: 1 - 12
# [요일]: 0 - 6 (0: Sunday, 1: Monday, ... , 6: Saturday)
```

#### 예시

- 매 분 실행

    ```zsh
    # 매분 test.sh 실행
    * * * * * /home/script/test.sh
    ```

- 특정 시간 실행

    ```zsh
    # 매주 금요일 오전 5시 45분에 test.sh 를 실행
    45 5 * * 5 /home/script/test.sh
    ```

- 반복 실행

    ```zsh
    # 매일 매시간 0분, 20분, 40분에 test.sh 를 실행
    0,20,40 * * * * /home/script/test.sh
    ```

- 범위 실행

    ```zsh
    # 매일 1시 0분부터 30분까지 매분 tesh.sh 를 실행
    0-30 1 * * * /home/script/test.sh
    ```

- 간격 실행

    ```zsh
    # 매 10분마다 test.sh 를 실행
    */10 * * * * /home/script/test.sh
    ```

### Cron 작업 로그 남기기

#### 로그 작성

```zsh
* * * * * /home/script/test.sh > /home/script/cron_test.log 2>&1
```

#### 로그 누적 작성

```zsh
* * * * * /home/script/test.sh >> /home/script/cron_test.log 2>&1
```

## 프로그램에서의 Scheduler

OS 뿐만 아니라 프로그래밍 언어나 프레임워크에서도 이와 같은 scheduler 기능을 제공한다.

Spring 프레임워크의 경우, Spring Schduler와 Spring Quartz가 많이 사용된다.

- Spring Scheduler
    - Spring Boot Starter의 기본적인 의존성으로 사용 가능하여 별도의 의존성을 추가할 필요가 없다
    - 간단한 어노테이션만으로 스케줄링 작업을 설정할 수 있다
- Spring Quartz
    - 라이브러리에 대한 의존성 추가 설정이 필요하다
    - Spring Scheduler에 비해 설정 방법과 동작 구조가 복잡하다
    - Spring Scheduler에 비해 더 세밀하고 다양한 기능을 제공한다

이와 같은 언어, 프레임워크에서 제공하는 기능을 사용하면 서버에서 별도로 스케줄링 작업을 관리할 필요없이 애플리케이션 단에서 스케줄링 작업을 처리할 수 있다. 또한 로깅이나 예외 처리와 같은 설정들을 좀 더 편하고 일관되게 진행할 수 있다는 장점이 있다.

## 참고

- [위키백과 - cron](https://ko.wikipedia.org/wiki/Cron)
- [리눅스 크론탭(Linux Crontab) 사용법](https://jdm.kr/blog/2)
- [[Linux]작업 예약 스케줄러(크론Cron)파일,자동 백업 명령 & 관련 문제](https://jhnyang.tistory.com/68)
- [[Spring] Scheduler 어떤걸 사용해야 할까 ? - Spring Scheduler와 Spring Quartz](https://sabarada.tistory.com/113)