= 환경의 구성을 Image로 만들어 두고, 이를 Container를 이용해 항상 동일한 환경을 제공해주는 platform

>	- Image = 환경 세팅
>	- Container = 실행 환경

### 애플리케이션의 컨테이너화
  
 : **애플리케이션과 그 애플리케이션이 실행되기 위해 필요한 모든 것을** 하나의 ==패키지(컨테이너)== 로 묶는 것







>	- Image로 Jenkins를 세팅하고, Container에서 Jekins를 구동함.
>
>	- Docker를 이용해 Jenkins를 세팅하게되면, Docker내부의 Docker를 만들어서 일정한 테스트 환경을 유지하는데 사용할 수 있음.

