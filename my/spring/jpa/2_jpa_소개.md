# JPA 소개

## 1. JPA (Java Persistence API)

- Java 진영의 ORM 기술 표준

## 2. ORM이란?

- Object Relational Mapping (객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재한다.

## 3. JPA의 동작 원리

### 3.1 JPA는 어플리케이션과 JDBC 사이에서 동작한다.

![Untitled](https://user-images.githubusercontent.com/37948906/145823440-8688c0ee-8409-4be8-9281-313dc0993a84.png)


### 3.2 저장

![Untitled 1](https://user-images.githubusercontent.com/37948906/145823871-e4c2ff71-efbe-4618-9884-e8ee4ec574bc.png)

### 3.3 조회
![Untitled 2](https://user-images.githubusercontent.com/37948906/145823435-36e8760f-b016-4b22-8919-c27fc5fae8d5.png)


## 4. JPA 변천사

EJB (엔티티 빈 - 자바 표준) - 너무 불편해!

하이버네이트 (오픈소스) - 하이버네이트 오픈소스를 만듦.

JPA (자바 표준) - 오 괜찮은데? 자바 표준으로 가져오자. (하이버네이트 개발자랑 코드를 가져와서 표준으로 만듦)

## 5. JPA는 표준 명세

- JPA는 인터페이스의 모음이다.
- 3가지 구현체가 있음. (하이버네이트, EclipseLink, DataNucleus)

## 6. 왜 JPA를 사용해야 하지?

### 6.1 SQL 중심적인 개발에서 객체 중심으로 개발

### 6.2 생산성

- 저장: `jpa.persist(member)`
- 조회: `Member member = jpa.find(memberId)`
- 수정: `member.setName("변경할 이름")`
- 삭제: `jpa.remove(member)`

### 6.3 유지보수

- 기존에는 필드를 변경하면 모든 SQL을 수정했지만 이제 필드만 추가하면 SQL은 JPA가 알아서 처리한다.

### 6.4 패러다임의 불일치 해결

- JPA와 상속
    
    ![Untitled 3](https://user-images.githubusercontent.com/37948906/145823437-645098e5-cb5e-44fa-ba04-4b9f0e5bb6a7.png)

    
    - `jpa.persist(album)`
    
    ```sql
    INSERT INTO ITEM ...
    INSERT INTO ALBUM
    ```
    
    - `jpa.find(Album.class, albumId)`
    
    ```sql
    SELECT I.*, A.*
    FROM ITEM I
    JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID
    ```
    
    - 위와 같이 Item을 상속받은 Album이 있다고 할 때, 개발자는 객체의 상속관계 그대로 코드를 짜도 JPA가 알아서 SQL을 만들어준다.
- JPA와 연관관계
    - 연관관계 저장
    
    ```sql
    member.setTeam(team);
    jpa.persist(member);
    ```
    
- JPA와 객체그래프 탐색
    - 객체 그래프 탐색
    
    ```sql
    Member member = jpa.find(Member.class, memberId);
    Team team = member.getTeam();
    ```
    
    - 신뢰할 수 있는 엔티티와 계층
    
    ```java
    class MemberService {
    	...
    	public void process() {
    		Member member = memberDAO.find(memberID);
    		member.getTeam(); // 믿을 수 있는 객체 그래프 탐색
    		member.etOrder().getDelivery();
    	}
    }
    ```
    
- JPA와 비교하기
    - 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장한다.
    
    ```java
    String memberId = "100";
    Member member1 = jpa.find(Member.class, memberId);
    Member member2 = jpa.find(Member.class, memberId);
    
    member1 == member2; // 둘은 같음.
    ```
    

### 6.5 JPA의 성능 최적화 기능

### 6.5.1 1차 캐시와 동일성(identity) 보장

1. 같은 트랜잭션 안에서는 같은 엔티티를 반환한다. - 약간의 조회 성능이 향상됨.
2. DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장

```java
String memberId = "100";
Member m1 = jpa.find(Member.class, memberId); // SQL
Member m2 = jpa.find(Member.class, memberId); // 캐시

println(m1 == m2) // true
```

### 6.5.2 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)

- INSERT
    - 트랜잭션을 커밋할 때까지 INSERT SQL을 모은다.
    - JDBC BATCH SQL 기능을 사용해서 한번에 SQL을 전송한다.
    
    ```java
    transaction.begin(); // 트랜잭션 시작
    em.persist(memberA);
    em.persist(memberB);
    em.persist(memberC);
    // 여기까지 insert sql을 데이터베이스에 보내지 않는다.
    // 커밋하는 순간 데이터베이스에 insert sql을 모아서 보냄.
    transaction.commit(); // 트랜잭션 커밋
    ```
    
- UPDATE
    - UPDATE, DELETE로 인한 로우(ROW)락 시간 최소화
    - 트랜잭션 커밋 시 UPDATE, DELETE SQL을 실행하고 바로 커밋
    
    ```java
    transaction.begin(); // 트랜잭션 시작
    changeMember(memberA);
    deleteMember(memberB);
    비즈니스_로직_수행(); // 비즈니스 로직 수행 동안 DB 로우 락이 걸리지 않는다.
    
    // 커밋하는 순간 데이터베이스에 UPDATE, DELETE SQL을 보낸다.
    transaction.commit(); // 트랜잭션 커밋
    ```
    

### 6.5.3. 지연 로딩 (Lazy Loading)

- 지연 로딩: 객체가 실제 사용될 떄 로딩
- 즉시 로딩: Join SQL로 한번에 연관된 객체까지 미리 조회

**지연로딩**

```java
Member member = memberDAO.find(memberId);
Team team = member.getTeam();
String teamName = team.getName();
```

**즉시로딩**

```java
Member member = memberDAO.find(memberId);
Team team = member.getTeam();
String teamName = team.getName();
```

### 6.6. 데이터 접근 추상화와 벤더 독립성

- JPA를 사용하면 벤더 종속적인 SQL을 JPA가 모두 만들어주기 때문에 Database 벤더 독립성을 가질 수 있다.

### 6.7. 표준

- JPA는 java 표준이다.

## 참고자료
- [김영한] ORM JPA Basic 강의 
- https://www.inflearn.com/course/ORM-JPA-Basic