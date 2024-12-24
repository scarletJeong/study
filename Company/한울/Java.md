
#1
>DAO - IBatis - DAO
		sql장소 :  sqlmap - example - sample
-
	VO - MyBatis + MVC2 
		sql장소 : sqlmap - exmaple - mappers


#2
 > jQuery / json / ajax
 >
 > 1.  jQuery  : javaScript 기반으로 만들어진 라이브러리  $\rightarrow$  UI화면 개발에 강력한 jsp
 > 2.  jSon : 인터넷에서 자료를 주고받을 때 쓰는 경량의 데이터형식 $\rightarrow$  1 : 1 데이터형으로 전송/취득할 수 있는 데이터표기법
 > 	- (name - value) 형태의 set으로 이루어짐.
 > `JSON : JavaScript Object Notaion `
 > 3. ajax :  비동기로 데이터를 주고받는 방법


#3
>Json Parse   $\rightarrow$  JsonObject  / JsonArray
>
>1. JsonObject  
>	- 1개 이상의 key-value쌍을 { } 중괄호로 이용하여 담은 객체
>	- 순서가 구분되지 않음
>	- key와 value 간의 구분은 ;(콜론)으로, 묶음간의 구분은 ,(컴마)로 함.
>	
>2. JsonArray
>	-...
>		
>JSON.Parse() : string객채  $\rightarrow$  JSON객체
>JSON.stringfy() : JSON객체  $\rightarrow$  string객체


#4
>MVC pattern
>	web Brower $\rightarrow$  Controller  $\rightarrow$  Service $\rightarrow$   DAO  $\rightarrow$  DB
>	
>	- Controller : 웹 브라우저의 요청을 전담하여 출력
>	- Service : 비즈니스 로직 수행 , DB에 접근하는 DAO를 이용해서 결과값을 받아옴
>	- DAO : DB에 접속하여 비즈니스로직 실행에 필요한 쿼리 호출
>	- DB : DB에서 알맞은 쿼리실행 후 결과값 반화
>	+ DTO _ Data Transfer Object
>		: 각 계층(view, controller, service..)이 데이터를 주고받을 때 사용하는 객체

<details> 
	<summary >  BoardDTO.java </summary>
	public class BoardDTO{
>		private int boardidx;
>		private String title;
>		...
>	}
	</details>
 
 <details> 
	<summary > Controller.java </summary>
	@Controller
	public class BoardController{
	
			@Autowired
			private BoardService boardService;

			@RequestMapping(value="/apple.do" )
			public ModelandView boardList( ) throws Exception{
				ModelAndView mv = new ModelAndView("/apple/boardList.do");
				List BoardDTO> list = boardService.selectBoardList();
				mv.addObject('List', list);

				return mv;
			}
	}
	</details>


> Service =  Service + ServiceImpl
> `Service_Impl : service interface를 구현한 "CLASS" `

<details> 
	<summary >  BoardService.java </summary>
	public interface BoardService{
>		List BoardDTO> selectBoardList() throws Exciption;
>	}
	</details>

<details> 
	<summary >  BoardServiceImpl.java </summary>
	public  class BoardServiceImpl implements BoardService {
>			
>			@Autowired
>			private BoardMapper boardMapper{
>	
>				@Override
>				public List BoardDTO> selectBoardList() throws Exciption{
>					return boardMapper.selectBoardList();
>				} 
>			}		
>	}
>	
	</details>

<details> 
	<summary >  BoardMapper.java </summary>
	
	@Mapper
	public interface BoardMapper{
>		 List BoardDTO> selectBoardList() throws Exciption;
>	}
	</details>

<details> 
	<summary > boardSQL.xml </summary>

	mapper namespace = "board.board.mapper.BoardMapper">
		select id = "selectBoardList" resultType = "board.board.dto.BoardDTO">
				<![CDATA[
						SELECT
								board_idx, title
						FROM t_table
							WHERE deleted_yn = 'N'
							ORDER BY board_idx DESC
				]]>
				...
				
		/mapper>
		
	</details>


> + @Controller 
> 	- 이 anotation으로 여긴 컨트롤러 클래스야 라고 알려줌
> 	- 사용자요청이 들어오면 이 컨트롤러가 호출됨.
> 
> + @Service 
> 	- 이 anotation으로 이 클래스가 서비스 클래스라는 것을 알려줌
> 
> + @Mapper 
> 	- 이 anotation으로 Mapper 인터페이스라고 인식함.
> 	- ?.xml 형식의 파일을 만들어 원하는 SQL문을 작성함.
> 
> + namespace
> 	- mapper의 전체경로
> 	
> + id : 매퍼 인터페이스와 XML파일을 매칭시키기위해 매퍼 인터페이스의 메소드명과 XML파일의 id를 동일하게 작성해야 함.
> + resultType : SQL을 실행하고, 결과값을 어떤 형식으로 반환할지 나타냄.


#5
> DAO vs VO vs Repository
> 
> - DAO _ Data Access Object
> 	- repository 패키지
> 	- DB의 데이터에 접근하기위한 객체 삽입/삭제/조회 등의 기능 수행
> 	- JPA에서의 Repository의 기능과 동일한 역할
> 
> - DTO _ Data Transfer Object
> 	- dto 패키지
> 	- 각 계층간 (controller, view, Business Layer(Model)) 데이터 교환을 위한 객체
> 	- 로직을 가지지 않는 데이터 객체
> 	- getter / setter 메서드만 가진 클래스
> 	 
> - VO _ Value Object
> 	- dto 패키지
> 	- 불변, read-only 클래스
> 	- setter성격을 가지고 있는 메서드를 가지면 안됨.
> 		오로지 생성자로만 값을 초기화해야하고, getter성격의 메서드만 사용
> 
> - Repository
> 	- 영구저장소 x $\rightarrow$ 객체의 상태를 관리하는 저장소
> 	- 인터페이스로 구현체를 따로 구현하는 도메인기술 or 서비스 계층
> ---------------------------------
> `DTO : 계층간 데이터 전달용
> `VO : 값을 갖는 도메인
> `Entity : DB와 매핑되는 용
> `Repository = DAO = DB점근용`


#6
> MAVEN
> : 자바용 프로젝트 관리도구
> - Apache Ant의 대안으로 만들어짐.
> - Maven은 필요한 라이브러리를 특정문서(pom.xml)에 정의해 놓으면, 내가 사용할 라이브러리뿐만 아니라 해당 라이브러리가 작동하는데 필요한 다른 라이브러리까지 관리하여 네트워크를 통해 자동으로 다운받음
> - Maven은 중앙 저장소를 통한 '자동 의존성 관리'와 중앙저장소는 라이브러리를 공유하는 파일서버로 볼 수 있음.
> - Maven은 자기회사만의 중앙 저상소를 구축할 수 있음.
> 
> 
> + 빌드(build)
> 	- 소스코드 파일을 컴퓨터에서 실행할 수 있는 독립 소프트웨어로 변환하게하는 과정
> 	- 작성한 소스코드(java), 프로젝트에서 쓰인 각각의 파일 및 자원 (.xml. .jpg, .jar, .properties)을 JVM이나 톰캣캍은 wAS가 인식할 수 있는 구조로 패키징하는 과정 및 결과물
> 	- -
> + 빌드 도구_build tool
> 	- 프로젝트 생성, 테스트 빌드. 배포 등의 작업을 위한 전용 프로그램
> 	- 빠른 기간동안 계속해서 늘어나는 라이브러리 추가와 프로젝트를 진행하며 라이브러리 버전 동기화의 문제를 해소하고자 등장
> 	- 초기 java의 빌드도구 : Ant
> 	- 최근 : Maven, Gradle..


#7
> JPA : Java Persistence Api
> 	: 자바 진영에서 ORM기술표준으로 사용되는 인터페이스의 모음
> 		 `+인터페이스의 모음 = 실제 구현된긋이 아니라 구현된 클래스와 매핑해주기위해 사용되는 framework. 
> 		 `대표적인 JPA : Hibernate`
>


#8
> ORM : Object- Relational Mapping
> 	: 어플리케이션 class와 RDB(Relation Data Base)의 테이블을 매핑(연결)함
> 		- 어플리케이션의 객체를 RDB table에 자동으로 영속화해줌. 
> 			`영속화 : 어떤 일이 중도에 끊기거나 바뀌게 하지 않고 지속됨.` 


#9
> Mapper interface / DAO 
> - Mapper interface를 사용하지 않았을 때
> -  <>mapper namespace = "userNS../>     
> 	- mapper 인터페이스를 사용하지 않으면, SQL를 호출하는 프로그램은  sqlSession 메서드의  argument에 문자열로 지정해야하는데, 이는 문자열로 지정하기 때문에 오타로 인한 에러가 발생한다. 
> 
> -  Mapper interface를 사용했을 때 
> - <>mapper namespace = "myspring.user.dao.UserMapper".../>
> - public inerface UserMapper{}
> 	-  UserMapper 인터페이스는 개발자가 작성하면 됨.


+
>  Mapper Factory Bean 설정하기 - bean설정파일
>  $\rightarrow$ Mapper Factory Bean은 sqlSessionFactory 나 sqlSessionTemplate를 필요로 함.
>  $\rightarrow$ 프록시는 런타임에 생성되기때문에 지정된 mapper는 실제 구현 클래스가 아닌, 인터페이스여야 한다.
>  
>  <>bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
> 		 <>property name = "mapperInterface" value = "mySpring.user.dao.UserMapper"/>
> 		 <>property name = "sqlSessionTemplate" ref = "sqlSession" />
> <>bean/>


#10
> config fiile
>  - Spring  설정 파일
> 	 - a. spring framework
> 		 - context-aspect.xml  :  AOP관련 설정 정보를 관리
> 		 - context-common.xml :  messageSource, Trace 등 공통 설정 정보 관리
> 		 - context-datasource.xml : Data Source 설정 정보 관리
> 		 - contesxt-mapper.xml : sqlSession, Handler 등 MyBatis관련 설정 정보를 관리 
> 		 - context-transaction.xml : 트랜잭션 설정 정보 관리
> 
> 	- b. springMVC
> 		- dispatcher - servlet.xml  : dispatcher Servlet과 springMVC관련 공통 설정 정보를 관리


#11
> 자바 문자열 합치기
> 	1. '+' 연산자 
> 	2. concat() method
> 	3. append() method
> 	-
> -
> But, '+' 연산자를 이용해서 문자열 추가(합치기)는 지양함.
> 	- 왜?_ Java에서 String '+'연산자는 Java컴파일에서 구현되며
> 		컴파일 타임에  컴파일 전 내부적으로 StringBuilder클래스를 만든 후 다시 문자열로 반환함.
> 		
		$\rightarrow$ 성능 저하 및 메모리 낭비가 심해짐
			`단순한 + 연산의 호출에 StringBuilder객체가 생성되고, append() 와 toString() 메서드들도 매번 호출되어 성능저하와 메모리 낭비가 심해짐. `