`의무사 front`

#1
> 연결 Error -  @CrossOrigin(Origins = "*", maxAge=3600)
> $\rightarrow$ CORS : Cross- Origin Resource Sharinng
> : 교차 출처 리소스 공유
> - 외부(다른 도메인 서버)에서 자원소스들이 접근하는(=크로스 도메인)체계
> - 페이지 생성 시, 다른 출처의 자원(css, html, js 등)이 필요할 때 브라우저는 이미 SOP으로 인해 다른 출처와의 상호작용이 제한되어 있음. 이때 CORS는 HTTP헤더를 이용해, 한 출처에서 실행중인 웹 어플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여함.


#2
>실행  ok + 404 Error _ 특히  ajax에서 Error
>> $\rightarrow$ @ResponseBody  어노테이션이 없어서 발생한 에러. 
>> 이 어노테이션 없이 스프링은 리턴값을  view의 이름으로 받아들임, return "index"같이..
>> but, view의 이름이 long타입이 될 수 없기데 에러가 난거임. 
>>
>> `Annotaion that indivates a method return value should be bound to the Web response body. Support by annotated handler method.
>>  `: 리턴값이 view이름이 아닌 responsebody에 귀속되는데 이것을 알리는게 @Responsebody어노테이션.`


#3
> Xml 파일 $\rightarrow$  js로 바뀐 다음에 js를 호출서 사용함. because of   속도 개선
> Js로 build된게 안되어서 화면에 안뜨고 경로문제 생기는 것. so, Js build경로를 잡아줘야한다. 
> 처음 xml 경로를 잡아주는 것 처럼, 
> :: web square경로문제는 파일경로를 잡아주면 된다. 


#4
> 'HTTP Body is `&rightarrow;`  '  Missing' Error
	$\rightarrow$ @GET / @POST  방식때문
		-  GET/POST : HTTP method로 클라이언트에서 서버로 무언가를 요청할 때 사용함.
		- GET : get을 통한 요청은  URL주소 끝에 (= body가 필요없다 ) 파라미터를 포함되어 전송되며, 이거을 쿼리 스트링(query string)이라 부름. 
		-  POST : post를 통한 요청은 HTTP메시지 Body부분에 담아서 서버로 보낸다.
		- 
		so, submisssion 설정 시, GET으로 설정하면  requestBody를 쓰기위해서는 method를 POST로 해야한다. 


