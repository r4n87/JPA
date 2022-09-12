# JPA
- [Section 01. What is JPA](#section-01-what-is-jpa)
- [Section 02. Start JPA](#section-02-start-jpa)
- [Section 03. 영속성 관리](#section-03-영성-관리)

## Section 01. What is JPA
### 설명
JPA
- Java Persistence API
- ORM 기술 표준

ORM
- Object-relational mapping 객체 관계 매핑
- 객체 / 관계형 데이터베이스를 각각 설계하고 ORM 프레임워크가 중간에서 매핑

JPA 동작
1. 저장
- DAO > Entity Object > JPA > JDBC API > DB
- JPA는 Entity를 분석하고 패러다임의 불일치 해결

2. 조회
- JPA가 멤버 객체를 보고 Select SQL 생성
- ResultSet 매핑


JPA 소개
- EJB 엔티티 빈 (자바 표준)
- 하이버네이트 (오픈 소스)
- JPA (자바 표준)

JPA를 사용해야 하는 이유
- SQL 중심 개발 > 객체 중심 개발
- 생산성: 간단한 function을 통해 CRUD 가능
- 유지보수: 필드만 추가하면 됨. SQL은 JPA가 처리
- 패러다임의 불일치 해결
  - JPA와 상속, 연관관계, 객체 그래프 탐색, 비교하기
  - 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장
- 성능
  - 1차 캐시와 동일성 보장
    : 같은 트랜잭션 안에서는 같은 엔티티를 반환 > 조회 성능 향상  
      DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장  
  - 트랜잭션을 지원하는 쓰기 지연
    : 트랜잭션을 커밋할 때까지 INSERT SQL을 모음  
      JDBC BATCH SQL 기능을 사용해서 한 번에 SQL 전송  
      transaction.begin(); > SQL > transaction.commit();  
  - 지연 로딩
    : 객체가 실제 사용될 때 로딩  
    (<-> 즉시 로딩: JON SQL로 한 번에 연관된 객체까지 미리 조회 / jpa 옵션으로 가능)  
- 데이터 접근 추상화와 벤더 독립성
- 표준


## Section 02. Start JPA
### 실습
H2 데이터베이스
- 최고의 실습용 DB
- 가벼움
- 웹용 쿼리툴 제공
- MySQL, Oracle 시뮬레이션 가능
- 시퀀스, AUTO INCREMENT 기능 지원

개발 환경
- IntelliJ
- Java 8.0 이상
- Maven
- Dependency 설정

```
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-entitymanager</artifactId>
    <version>5.3.10.Final</version>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>1.4.199</version>
</dependency>
```

- persistence.xml 생성
```
<?xml version="1.0" encoding="UTF-8" ?>
<persistence version = "2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.rog/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
    <persistence-unit name="Hi">
        <properties>
            <!-- 필수 속성 -->
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver" />
            <property name="javax.persistence.jdbc.user" value="sa" />
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
            <!-- 옵션 -->
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
            <property name="use_sql_comments" value="true"/>
        </properties>
    </persistence-unit>
</persistence>
```

객체와 테이블 생성 및 매핑
- @Entity: JPA가 관리할 객체
- @Id: 데이터베이스 PK와 매핑

CRUD
- 조회: em.find
- 수정: set
- 삭제: remove(객체)
- em.createQuery(쿼리).getResultList를 통해 다량 조회 가능 / 대상은 테이블이 아닌 객체

주의할 점
- 엔티티 매니저 팩토리는 하나만 생성해서 애플리케이션 전체에 공유
- 엔티티 매니저는 쓰레드간에 공유 X
- JPA의 모든 데이터 변경은 트랜잭션 안에서 실행

JPA
- 엔티티 객체 중심 개발
- 검색 시에도 테이블이 아닌 엔티티 객체를 대상으로 검색
- 모든 DB 데이터를 개체 변환하여 검색하는 것은 불가능
- 검색 조건이 포함된 SQL이 필요

JPQL
- 객체 대상으로 검색하는 객체 지향 쿼리
- SQL을 추상화해서 특정 데이터베이스의 SQL에 의존하지 않음
- 객체 지향 SQL

## Section 03. 영속성 관리
### 정리
비영속
- 객체를 생성한 상태
- JPA와 관계가 없음. DB에 들어가지 않음

영속
- 객체를 저장한 상태
- em.persist를 수행하는 순간 관리가 시작됨
- 영속 상태가 된다해도 DB에 바로 쿼리가 날아가지 않음
- transaction을 commit할 때 쿼리가 날아감

준영속
- detach: 엔티티를 영속성 컨텍스트에서 분리

삭제
- remove

영속성 컨텍스트의 이점
- 1차 캐시
- 동일성(identity) 보장
- 트랜잭션을 지원하는 쓰기 지연
- 변경 감지

1차 캐시에서 조회
- 조회를 했는데 select 쿼리가 나가지 않음
- persist 시 1차 캐시에 저장되어 있던 객체가 조회됨

영속 엔티티의 동일성 보장
- 반복 가능한 읽기 등급의 트랜잭션 격리 수준을 애플리케이션 차원에서 제공

트랜잭션을 지원하는 쓰기 지연
- 엔티티 등록 시 지원하는 사항
- 트랜잭션을 commit하는 순간 데이터베이스에 sql을 보냄

엔티티 수정 변경 감지
- dirty checking
- flush 시 Entity와 스냅샷을 비교함

플러시
- 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영
- commit 시 일어남
- 영속성 컨텍스트를 비우지 않음
- 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화
- 트랜잭션이라는 작업 단위가 중요 > 커밋 직전에만 동기화하면 됨

플러시 발생
- 변경 감지 (dirty checking)
- 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
- 저장소의 쿼리를 DB에 전송

준영속 상태
- detach() 수행
- JPA에서 관리하지 않음
- transaction을 commit할 때 관련 반응이 일어나지 않음

준영속 상태로 만드는 방법
- detach: 특정 엔티티만 준영속 상태로 전환
- clear: 영속성 컨텍스트를 완전히 초기화
- close: 영속성 컨텍스트 종료
