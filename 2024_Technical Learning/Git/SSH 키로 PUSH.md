

`-` ssh 키 생성
	`ssh-keygen -t rsa -b 4096 -C "scarletJeong@example.com"`

 - ssh키 형태가 2개인듯
	공개 키는 `id_rsa.pub`이고, 비밀 키는 `id_rsa`

`-` ssh 키 복사
	`cat ~/.ssh/id_scarlet.pub`
	
`-` ssh키가 여러개면 config에서 관리해야함
	 - `.ssh` 폴더가 있는지 확인
		`ls ~/.ssh`
	-`.ssh` 폴더 생성
		`mkdir ~/.ssh`
	-`.ssh` 폴더 안에 `config` 파일이 없는 경우 새로 생성 + config 작성
		`notepad ~/.ssh/config`
	- config 확인 (터미널용)
		`cat ~/.ssh/config`
		`type C:\Users\repure\.ssh\config`
	- `Host` 이름이 제대로 작성되었는지 확인하기 위해 `ssh` 명령으로 디버깅
		`ssh -v git@github.com-scarletJeong`
	- 캐시 초기화
		`ssh-add -D`
	-( SSH 클라이언트를 재시작하여 설정을 적용 )
		`Get-Service ssh-agent | Start-Service` 
	- SSH 연결 테스트
		`ssh -T git@github.com-scarletJeong`


`-` Git 리모트 URL 설정
	- SSH 사용하도록 Git 리모트 재설정
		`git remote set-url origin git@github.com-scarletJeong:scarletJeong/python.git`
	- 설정 확인
		`git remote -v`
			> `origin  git@github.com-scarletJeong:scarletJeong/python.git (fetch)`
			` origin  git@github.com-scarletJeong:scarletJeong/python.git (push)`	
	- Git Push
		`git push -u origin main`
			- 강제 push
			`git push -u secondary main --force `




```
config 파일

	# Example configuration
	Host github.com-scarletJeong
	    HostName github.com
	    User git
	    IdentityFile ~/.ssh/id_scarlet
	    IdentitiesOnly yes
    
    
    ex
    # 첫 번째 키 설정
	Host github.com-repureJiyeon
	    HostName github.com
	    User git
	    IdentityFile ~/.ssh/id_rsa
	    IdentitiesOnly yes

	# 두 번째 키 설정
	Host github.com-scarletJeong
	    HostName github.com
	    User git
	    IdentityFile ~/.ssh/id_scarlet
	    IdentitiesOnly yes

```









`+` config 파일 권한
 `-` ".ssh/config" 파일이 읽기 가능하도록 설정이 되어 있는지 확인
	 `icacls "C:\Users\repure\.ssh\config"`
`-` 읽기 권한 부여 
	`icacls "C:\Users\repure\.ssh\config" /grant:r "%USERNAME%:(R)"`

`+` config 파일은 .txt 확장자 없음
	- 확장자 제거
		`Rename-Item -Path "C:\Users\repure\.ssh\config.txt" -NewName "config"`
	-확인
		`ls ~/.ssh`
		

`+` SSH 에이전트 확인
`ssh-agent가 실행 중인지 확인하고 SSH 키가 에이전트에 추가되었는지 확인`
`-` ssh-agent 실행
	`eval $(ssh-agent)`
`-` SSH 키 추가
	`ssh-add ~/.ssh/id_scarlet`
`-` SSH 에이전트에 추가된 키 확인
	`ssh-add -l`
		> `id_scarlet` 키가 나와야 함.
		
		

`+` known_hosts 파일 초기화
`-` 기존 `known_hosts` 초기화
	`rm ~/.ssh/known_hosts`
`-` 새로 GitHub 키를 추가
	`ssh -T git@github.com-scarletJeong`


`+` 연결된 SSH 키를 제거
	`ssh-add -d ~/.ssh/id_rsa`



`+`
```
PS C:\python> ssh -T git@github.com-scarletJeong
Hi scarletJeong! You've successfully authenticated, but GitHub does not provide shell access.

```