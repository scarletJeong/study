.
	- 빌드 파일은  Jar or war 2가지 존재.
	- Jar 
	  - Spring Boot를 사용하는 경우?
	  _ Boot 내부 서버가 내장되어있기에.
	  `Tomcat 또한 SpringBoot 가이드 기본 기준`
	- War
	  - JSP 만으로 이루어져있음
	  - 외장 WAS 를 사용하는 경우
	- 


방법1. CMD
	- 배포 서버 내부에서 작업해야 하는 경우
	  `+ ls == dir
		  `ls 는 리눅스와 macOS
	  
	  1. 작업하는 프로젝트 내부 진입
	     cd [Directory]
	    
	  2. 빌드 실행
	     ./ gradlew build 
	     
	  3. Jar 파일이 빌드되었는 Directory로 이동
	     cd ./build/libs
	     
	  4. Jar 파일 실행
	     java -jar [file-name]      
	    
		*Jar 빌드 시 파일이름은 biuld.gradle 파일내부 버전에 설정된 값이 적용된다.
		* ex ) 프로젝트명-0.0.1-SNAPSHOT.jar
	  

 + sudo lsof kill...



방법2. Intelli J 에서 만들기

![[인텔리제이에서자르파일만들기.png]]



참고 : 
https://isaac-christian.tistory.com/entry/AWS-Spring-Boot-%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%EC%9D%84-AWS%EC%99%80-Mobaxterm%EC%9C%BC%EB%A1%9C-%EC%84%9C%EB%B2%84%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0