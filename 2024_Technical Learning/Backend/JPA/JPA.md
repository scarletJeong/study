
`Java Persistence Api`
>	- 자바에서 DB와 상호작용할 때 사용되는 표준 **인터페이스**
>	- 객체 지향 프로그래밍 환경에서 DB작업을 쉽게 할 수 있도록 도와줌.
>	
>	- 객체와 관계형 DB의 데이터를 매핑하는 ORM`(Object-Relational Mapping)` 프레임워크의 일종
>	- 인터페이스를 통해 표준을 정의할 뿐, 실제 구현체(Hibernate)를 사용하여 기능을 실행함.
>	
>	
>	- 주요 개념과 기능
>		- 1. 객체와 테이블 매핑
>			- JPA는 자바 객체 (Entity)와 DB테이블을 매핑하여 SQL작성 필요 없이 데이터를 조작할 수 있음.
>			- @Entity, @Table 등의 어노테이션을 사용하여 클래스와 테이블을 매핑
>		- 2. 영속성 컨텍스트
>			- JPA는 엔티티를 저장/수정/삭제/조회 하는 작업을 영속성 컨텍스트라는 개념을 통해 관리함.
>			- 영속성 컨텍스트는 엔티티의 상태를 관리하며, 변경된 내용이 DB에 반영되도록 함.
>		- 3. CRUD 작업 간편화
>		- 4. JPQL _Java Persistence Query Language_
>			- DB에 종속적이지 않은 쿼리 언어인 JPQL을 지원
>			- 객체 지향 쿼리 언어로, SQL과 유사하지만 테이블 대신 엔티티를 대상으로 쿼리 작성함.
>		- 5. 캐시의 성능최적화
>			-







+ Hibernate
	> - 객체-관계 매핑(ORM) **프레임워크**
	> - 자바 객체를 관계형 데이터베이스 테이블에 매핑 + SQL 쿼리를 작성하지 않고 DB작업을 수행하도록 함.
	> - 자바 객체와 DB간의 변환을 자동화
	> 
	> 주요 기능
	> - 자동 매핑 
	> 	- 엔티티 클래스와 DB 테이블을 자동으로 매핑함.
	> 	- 자바 객체를 DB 테이블의 행과 컬럼으로 자동 변환
	> - 지연 로딩 > 성능 최적화
	> 	- 관계가 있는 데이터를 실제로 필요할 때까지 데이터베이스에서 로드하지 않음으로써 성능 최적화함.
	> - DB 독립성
	> 	- 데이터베이스에 종속되지 않고 여러 DBMS를 지원함.
	> 	- 애플리케이션 코드의 수정없이도 DB교체 가능
	> - HQL(Hiberate Query Language)
	> 	- SQL과 유사하지만 엔티티를 객체 기반으로 쿼리하느 Hibernate 전용 언어.
	> 	- 데이터베이스에 독립적이며, 객체 지향적인 쿼리 작성 가능.
	> 
	> 
	> Hibernate 사용방법
	> - Spring Boot와 함께 사용됨.
	> - `Spring Boot 에서 JPA설정을 간단하게 해주면 기본적으로 Hibernate설정을 많이 건드리지 않고 사용할 수 잇음._기본적으로 Hibernate를 사용하도록 설정되어있음`
	> 
	> 
	> <순서>
	> - 설정파일에 디펜던시 추가 > 엔티티 클래스 작성 > Repository 인터페이스 작성 > 애플리케이션 설정 > Service계층에서 사용
	> 
	> - 1. spring-boot-starter-data-jpa _ 디펜던시 추가
			- IntelliJ
				- build.gradle 파일 > 
					- dependencies{ implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
								  ...
								  }
			- Sping Boot
				- .xml 파일 > 
					- `<dependency> 
						`<groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-data-jpa</artifactId> 
					` </dependency> 
>
>
>	- 2. @Entity, @Table 어노테이션 사용
>		  - 데이터베이스와 매핑되는 엔티티클래스 작성
>		    
>	- 3. JpaRepository 인터페이스 상속하여 CRUD 구현
>		- Hibernate가 JPA의 구현체로 동작하여 실제 데이터베이스 작업을 수행함.
>		  
>	- 4. 애플리케이션 설정
>		- Intelli J
>			- applicataion.yml 파일 >
>				- spring:  
>					- datasource:  
>					-    url: jdbc:postgresql://repurehc.chayw4we4abo.ap-  northeast-.rds.amazonaws.com:5432/purewithme  
>					-    username: repurehc  
>					-    password: repuredev1!  
>					-    driver-class-name: org.postgresql.Driver
>		- Spring Boot
>			- application.properties 파일 >
>				- spring.datasource.url=jdbc:h2:mem:testdb spring.datasource.driverClassName=org.h2.Driver spring.datasource.username=sa spring.datasource.password=password spring.jpa.show-sql=true spring.jpa.hibernate.ddl-auto=update
>		
>	- 5. Repository를 사용사여 Service에서 엔티티 저장 및 조회
>	


#### 요약
- JPA : 자바에서 ORM을 위한 표준 **인터페이스
- Hibernate : JPA를 구현한 **ORM 프레임워크** 중 하나로, 자바 객체와 관계형 데이터베이스 간의 매핑을 실제로 구현
`Hibernate 외에도 EclipseLink, OpenJPA 등이 JPA를 구현한 다른 ORM 프레임워크들이지만, Hibernate가 가장 널리 사용됩니다.`