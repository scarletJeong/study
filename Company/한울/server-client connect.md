`10월 한울 본사에서 잠깐 했던것.`

10/25
client- server connect 중
System.StackOverflowExciption 형식의 첫째 예외가 발생했습니다.

발생이유
> 1. 무한호출 - 자기 자신 호출
> 2. 반복문에서 빠져나가는 조건이 없는 경우

나의 이유
> port 번호를 너무 크게줘서 IPParse하는데서 파싱을 못해 생긴 오류.

> "값 형식은 [Int16](https://learn.microsoft.com/ko-kr/dotnet/api/system.int16?view=net-7.0) 음수 32768부터 양수 32767까지의 값으로 부 서명된 정수입니다.
> 
> 이 형식은 이 형식의 instance 값을 문자열 표현으로 변환하고, 숫자의 문자열 표현을 이 형식의 instance 변환하고, 이 형식의 인스턴스를 비교하는 메서드를 제공합니다."
> 


> 그러므로 이 오류는 포트번호를 63040에서 6304로 줘서 해결함...

- [ ] 서버와 클라이언트 그리고 포트에 대해서 공부하기.
- [ ] dll 이 뭐지
> 