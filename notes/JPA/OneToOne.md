# OneToOne

- 1:1 관계
- 1:M, M:1 관계와는 달리 연관관계의 주인이 명확하지 않다.
- 연관관계의 주인을 어디에 둘지 고민 필요
    - 추후 1:M 관계로 변경될 가능성이 있는지 고민 -> 가능성이 있다면 M이 될 곳에 주인 할당
    - 이러한 판단을 하기 위해서는 도메인 지식 필요
- 주 테이블이 외래 키를 가지는 경우
    - Pros : 주 테이블만 조회해도 대상 테이블에 데이터가 있는지 알 수 있음
    - Cons : 외래키에 null을 허용해야 함
- 대상 테이블이 외래 키를 가지는 경우
    - Pros :
        - null 허용에 대한 문제가 없음
        - 관계가 1:1에서 1:M으로 변경 시 테이블 구조 변경이 없음
    - Cons : 
        - 코드 상에서 보통 주 테이블에서 대상 테이블을 조회하는 경우가 많기 때문에 어쩔 수 없이 양방향 매핑을 해야 함
        - 지연 로딩이 동작하지 않음, N + 1 쿼리 문제 발생 -> 관련하여 프록시 등 추가 학습


### Reference
- [[JPA] @OneToOne, 일대일[1:1] 관계](https://ict-nroo.tistory.com/126)
- [Spring Boot + JPA step-08: OneToOne 관계 설정 팁](https://www.popit.kr/spring-boot-jpa-step-08-onetoone-관계-설정-팁/)
- [JPA OneToOne?](http://wonwoo.ml/index.php/post/1530)
- [Spring boot :: JPA에서 OneToOne 관계 N+1 문제 정리](https://wave1994.tistory.com/156)
