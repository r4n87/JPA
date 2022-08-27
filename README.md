# JPA
- [Section 01. What is JPA](#section-01-what-is-jpa)
- [Section 02. Start JPA](#section-02-start-jpa)


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
### 설명
