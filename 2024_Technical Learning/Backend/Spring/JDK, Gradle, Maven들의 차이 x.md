JDK, Gradle, Maven들의 차이




### + application.properties와 application.yml 차이점
- Spring Boot에서 애플리케이션의 설정을 정의하는데 사용

1. application.properties
>	- 구조
>		- 키 - 값 쌍 형태로 설정을 정의
>		- ex)
>			`server.port=8080
>			`spring.datasource.url=jdbc:mysql://localhost:3306/mydb
>			`spring.datasource.username=root
>			`spring.datasource.password=root
>	- 특징
>		- 간단한 설정
>		- 설정항목이 많아질 경우 파일이 길어지고 관리가 어려움.
>		- Spring Boot 프로파일 - *파일 분리 필요*

2. application.yml
>	- 구조
>		- YAML 문법을 사용하여 계층구조 표현
>		- ex)
>			`server :
>				`port : 8080
>			`spring :`
>				`datasource :
>					`url : jdbc:mysql://localhost:8080/mydb
>					`username : root`
>					`password : root`
>	- 특징
>		- 설정이 복잡하거나 중첩된 경우 추천
>		- 여러 환경 설정(프로파일)을 쉽게 관리
>		- Spring Boot 프로파일 - *YAML 내부에서 선언 가능*


#### + 프로파일 관리

##### 1. `application.properties`에서의 프로파일 관리

- 환경별 설정을 별도의 파일로 분리해야 함.
> 	- 기본 파일
> 		- application.properties 
> 		→ 모든 환경에서 공통으로 사용하는 설정
> 	- 환경별 파일
> 		- application-dev.properties → 개발 환경 설정
> 		- application-prod.properites → 운영 환경 설정

- **Spring Boot가 프로파일을 인식하는 방법**
>	1. 기본 파일(`application.properties`):   
>		`spring.profiles.active=dev`
>		 활성화할 프로파일을 지정(`dev` 또는 `prod`).
>		 
>	2. 환경별 파일 
>		- 1. `application-dev.properties`:
>			`server.port=8080 
>			`spring.datasource.url=jdbc:mysql://localhost:3306/dev_db spring.datasource.username=dev_user 
>			`spring.datasource.password=dev_pass`
>		
>		- 2. `application-prod.properties`:  
>			`server.port=80 
>			`spring.datasource.url=jdbc:mysql://prod-db:3306/prod_db spring.datasource.username=prod_user 
>			`spring.datasource.password=prod_pass`       
>			
>	3. 프로파일 실행:
>		- Spring Boot 애플리케이션 실행 시, 개발 환경에서 실행:
>		`java -jar your-app.jar --spring.profiles.active=dev`


##### 2. `application.yml`에서의 프로파일 관리
- 파일 분리 없이 한 파일 내에서 여러 프로파일 설정 
```
spring:
  profiles:
    active: dev  # 활성화할 프로파일 설정


spring:
  profiles: dev
  datasource:
    url: jdbc:mysql://localhost:3306/dev_db
    username: dev_user
    password: dev_pass
  server:
    port: 8080


spring:
  profiles: prod
  datasource:
    url: jdbc:mysql://prod-db:3306/prod_db
    username: prod_user
    password: prod_pass
  server:
    port: 80

```