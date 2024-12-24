
``:프레임워크
 $\rightarrow$  `직접 작성하는 SQL, 저장 프로시저, 고급 매핑등을 지원하는 퍼시스턴스 프레임워크
	 `- JDBC 코드, 수동으로 세팅하는 파라미터, 결과, 매핑을 제거
	 `- 데이터베이스 레콛크에 원시타입과 Map인터페이스, 자바 POJO를 설정
	 `- 매핑을 위한 XML과 Annotation 사용가능


#1
>변수).equals("문자(열)")  $\rightarrow$  ("문자열").equals(변수)
> :  because of security. 우선순위가 뒤에 것을 체크함. 
> 

#2
> ROWID
> : 오라클에서 '데이터 주소'
> 	- 테이블 각 레코드(행)이 가지고 있는 고유의 주소(정확한 저장 위치)를 나타냄
> 	- DB전체에서 중복되지 않는 유일한 값
> 	- 데이터 객체 변환 테이블이나 인덱스같은 데이터 객체가 생성될 때 활용됨. 
> 	- `인덱스(Index)
> 		`- 데이터베이스에서 데이터의 위치정보를 가진 주소록 
> 		`- DB의 위치`
> 		`- 인덱스 생성 시 모든 블록을 다 읽지 않고, 자료가 있는 블록으로 찾아가 해당 블록만 읽음.


#3
> 임시저장 sql 
> $\rightarrow$  `1, 2, 3, 4 번의 칸에 임시저장을 누를 때마다, 한칸씩 insert 되고, 만약 모든 칸이 차게되면 마지막 칸인 4번에 계속 update되는것.
> 
> 
> 1.  case  ~ when (by 나)> <details> 
	<summary > case ~ when </summary>

		##case ~ when 
		update LISQ200
> 			SET
> 		re1_result  = (case 
> 									when re1_result is null 
> 									then #{tempRes} 
> 									when re1_result is not null
> 									then #{re1} 
> 							end),
> 		re2_result  = (case 
> 									when re2_result is null and re1_result is not null 
> 									then #{tempRes} 
> 									when re2_result is not null
> 									then #{re2} 
> 							end),
> 		re3_result  = (case 
> 									when re3_result is null and re1_result is not null and re2_result is not null 
> 									then #{tempRes} 
> 									when re3_result is not null
> 									then #{re3} 
> 							end),							
> 		re4_result  = (case 
> 									when re4_result is null and re3_result is not null 
> 									then #{tempRes} 
> 									when re4_result is not null
> 									then #{tempRes} 
> 							end),	
> 		where PKLIS200 = #{pkLisq};	
> 		</details>
> 
> 2. merge into   (by 이준규)<details> 
	<summary > merge into </summary>
	merge into LISQ200 A
		using(
			select 
				DECODE(re1_result, ' ' , 1, 0)
				+  DECODE(re2_result, ' ' , 1, 0)
				+  DECODE(re3_result, ' ' , 1, 0)
				+  DECODE(re4_result, ' ' , 1, 0) nullCnt
			from LISQ200
			)B
			on (A.rowid = B.rowid and PKLISQ200 = '890' )
			when mathced then
				update 
					set
						re1_result = DECODE(B.nullCnt, 4, 'Data', re1_result)
						, re2_result = DECODE(B.nullCnt, 3, 'Data', re2_result)
						, re3_result = DECODE(B.nullCnt, 2, 'Data', re3_result)
						, re4_result = DECODE(B.nullCnt, 0, 'Data', . '1' , re4_result)
	</details> 
> > 3.  case ~ when + update  (by  김준우 부장님)
 <details> 
	<summary > case ~ when + update</summary>

		## case ~ when + update
		select
			case 
				when re1_result is null 
> 				then #{re1_result} 
> 			when re2_result is null and re2_result is null
> 				then #{re2_result} 
> 			when re3_result is null and re2_result is null
> 				then #{re3_result}
> 			when re4_result is null and re3_result is null
> 				then #{re4_result}	
> 		else 're1_result'
> 		end as chk
> 	, a*
> 	from LISQ200
> 	where PKLIS200 = '3';
>
		update 
					set
						re1_result = DECODE(B.nullCnt, 4, 'Data', re1_result)
						, re2_result = DECODE(B.nullCnt, 3, 'Data', re2_result)
						, re3_result = DECODE(B.nullCnt, 2, 'Data', re3_result)
						, re4_result = DECODE(B.nullCnt, 0, 'Data', . '1' , re4_result)
		where PKLISQ200 = '3';
> 		</details>


#4
> NVL 함수


#5
> DECODE


#6
> </if> 동적태그
> iBatis의 isEqual, isNull, isEmpty,, 와 동일한 function
> 
> if 문 사용시 주의점 > "따옴표"주의!
> 	if test = "name.equals('kim') " ></if>  > X
> 	if test = 'name.equals("kim") ' ></if>  > O
> 	


#7
> <![CDATA]
> - 쿼리 작성 시 < , > , & 같은 특수기호를 사용하는 경우 xml에 그냥 사용하게 되면 태그로 인식하는 경우가 있음. 
> - so, '태그가 아니라. 실제 쿼리에 필요한 코드야' 라고 알려줘야함.
> `문자열이야 = xml 로 parser하지마 `
> - 그게 바로 C태그 역할


#8
> #{ }  vs  ${ }
> -
> - #{ }
> 	- 들어오는 데이터를 문자열로 인식 $\rightarrow$ 자동따옴표
> 	- preparedStatement 생성
> 		- 매개 변수 값 안전하게 설정
> 		- preparedStatement가 제공하는 set계열의 메소드를 사용하여 물음표(?)를 대체할 값을 지정
> 
> - ${ }
> 	- statement 매개변수 값 그대로 전달 $\rightarrow$ 따옴표 없음
> 	- statement 생성
> 	- 대표 : 테이블을 변수로 받기, order by 함수에서..


#9
> MyBatis Error 
> : java.lang.IllegalArgumentException : Method Statement collection does not contain value for ~
> 
> 1. mapper id가 다를 경우 
> `mapper 파일에 <select id="...">에 id와 mappper파일에 접근하는 java파일 (DAO...Service)에 적어놓은  id값이 다른 경우`
    2. Parameter와 bean의 필드명이 다른 경우
    3. mapper 파일에 정의된 namespace와 mapper파일에 접근하는 Java파일(DAO or Service)에서 호출하는 namespace가 다를 경우
> 4. MyBatis config파일에  mapper가 정의되어 있지 않거나, 스펠링이 틀린경우
> 5. mapper에 정의된 namespace 명칭이 같은 Application 내애 중복될 경우


#10
>  org.apache.catalina.core.ApplicationDispatcher.invoke 
>  SERVER :  서블릿 [JSP]을 Servlet.service() 호출이 예외를 발생시켰습니다.
>  $\rightarrow$  LIQ110의 sys_date같은 date type column등이 날짜 형식이아니여서 뿌지 못 해 나타나는 Error
>  쿼리에서 to_char 사용해서 형태 맞춰서 뿌리면 됨.


#11
> IBatis_sql_iterate<> = MyBatis_sql_foreach<>
	<details> 
		<summary >  MyIBatis_sql_foreach </summary>
		## MyIBatis_sql_foreach
		<>select id="list" parameterType = "java.util.map"  resultType = "java.util.map"
				select * 
				from tables
				where table_schema IN
					<foreach item = "dbList" open = "("  close = ")"
									  seperator = "," />
							#{item}
					</>froeach		
		</details>

<details> 
		<summary >  	IBatis_sql_iterate </summary>
		## IBatis_sql_iterate
		<>select id="list" parameterType = "java.util.map"  resultType = "java.util.map"
				select * 
				from tables
				where table_schema IN
					<foreach item = "dbList" open = "("  close = ")"
									  seperator = "," />
							#{item}
					</>froeach		
		</details>
