`mobaXterm을 사용해서 서버에 자르파일 업로드 및 서버 온/오프`

- Mobaxterm 
	> 리눅스 환경의 SSH접속, FTP,  SFTP 등을 이 프로그램 하나로 다 사용가능
	> `원격 접속을 위해서는 여러기자 원격접속용 프로그램을 설치해야함.`
	> 
	> - SSH  _Secure SHell_
	> 	- 원격 접속 프로토콜
	> 	- 네트워크 상, 다른 컴퓨터에 로그인 혹은 원격 접속 등을 해주는 프로토콜.
	> 	   ` SSH 클라이언트 프로그램으로 putty, 파일질라, Git Bash 가 있음`
	> 	   `AWS 클라우드에 서버 설치 후, 내 컴퓨터에서 클라우드 서버에 접속하고 싶을 때`
	> 	   
	> - FTP    _File Transfer Protocol_
	> 	- 원격 파일 전송 protocol
	>   
	> - SFTP   _Secure File Transfer Protocol_
	> 	 - 원격 파일 전송 프로토콜
	



<설치 방법> 
2. Session → SSH 작성
   >- Remote host : 원격 접속할 ip/’도메인’ → 싸피 제공의 경우 [i6a604.p.ssafy.io/](http://i6a604.p.ssafy.io/)
   >   - username : ubuntu
   >   - advanced SSH settings → Use private Key에서 pem키 등록
   > 						  서버의 SSH 키를 .pem형식으로 로컬에 다운해서 꼭 넣어야 함.

3. 접속하여 mysql, nginx 설치
	 참고 : https://chaarles.tistory.com/21](https://chaarles.tistory.com/21) 


4. mysql과 workbench 연결
>	- ubuntu에서 mysql 접속 : sudo /usr/bin/mysql -u root -p
>	-  root이름으로, 외부 모든 곳에서 접근 가능한 유저 생성 
>	    (원래 root는 'root'@'localhost'임) & 모든 권한 부여
>	    `mysql> create user 'root'@'%' identified by '[password]';
>	     `mysql> grant all privileges on *.* to 'root'@'%' with grant option;`

참고 :  https://sutechblog.tistory.com/94 



###  MobaXterm 사용해서 서버에 JAR파일 올리기
> 서버의 IP 확인 → 서버 확인/종료 → JAR파일 업로드? → 서버 재시작
> 	  1. ip addr show
> 		  `서버의 모든 네트워크 인터페이스와 IP주소 확인`
> 	
> 	 2-1. ps aux | grep java 
> 		   `예시 출력:
> 		   `USER   PID %CPU %MEM VSZ    RSS TTY STAT START TIME COMMAND #          ubuntu 1234 0.2 1.2 3403448 40124 ?  Sl  10:00 0:10 java -jar myapp.jar`
> 	  2-2. sudo lsof -i :8080
> 		 `포트 8080을 사용하는 프로세스 확인
> 	  
> 	  3. sudo kill -9 1234
> 		  `PID 1234인 프로세스를 종료`
> 		  `위 명령에서 `-9`는 SIGKILL 신호를 의미하며, 해당 프로세스를 강제 종료.`
> 	  
> 	  4. nohup java -jar /home/username/myapp.jar &
> 	    `nohup java -jar purewithme/purewithme-0.0.1-SNAPSHOT.jar &`
> 	    
>       5. tail -f nohup.out  
> 	     `실행 로그 확인` 



참고 : 
https://exuzii.tistory.com/entry/Spring-Boot-41-%EC%84%9C%EB%B2%84-%EB%B0%B0%ED%8F%AC-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95-%EB%B0%B0%ED%8F%AC-%ED%8C%8C%EC%9D%BC-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EC%A0%84%EC%86%A1