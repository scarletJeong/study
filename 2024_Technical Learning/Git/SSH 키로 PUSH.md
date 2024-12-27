# SSH 키 관리와 Git 리모트 설정

## 개요
SSH 키는 안전하고 암호화된 방식으로 원격 서버와 통신하기 위한 필수 도구임. 특히 Git과 같은 버전 관리 시스템에서 SSH 키를 활용하면 비밀번호 없이도 인증과 작업이 가능해짐. 본 문서는 SSH 키 생성, 관리, Git 리모트 설정 과정을 다룸.

---

## SSH 키 생성
### 1. 키 생성
```bash
ssh-keygen -t rsa -b 4096 -C "scarletJeong@example.com"
```
- 공개 키(`id_rsa.pub`)와 비밀 키(`id_rsa`)가 생성됨.

---

## SSH 키 관리

### 1. 키 복사
```bash
cat ~/.ssh/id_scarlet.pub
```
- 공개 키를 복사하여 원격 서버나 서비스(GitHub 등)에 등록.

### 2. 여러 SSH 키 관리
1. `.ssh` 폴더 확인 및 생성
   ```bash
   ls ~/.ssh
   mkdir ~/.ssh
   ```
2. `config` 파일 생성 및 작성
   ```bash
   notepad ~/.ssh/config
   ```
3. `config` 파일 예제
   ```
   Host github.com-scarletJeong
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_scarlet
       IdentitiesOnly yes
   ```
4. SSH 연결 테스트
   ```bash
   ssh -T git@github.com-scarletJeong
   ```

5. SSH 에이전트 재시작 및 캐시 초기화
   ```bash
   ssh-add -D
   Get-Service ssh-agent | Start-Service
   ```

6. SSH 에이전트에 키 추가
   ```bash
   ssh-add ~/.ssh/id_scarlet
   ssh-add -l
   ```

7. `known_hosts` 초기화 및 GitHub 키 추가
   ```bash
   rm ~/.ssh/known_hosts
   ssh -T git@github.com-scarletJeong
   ```
                                   
`+`
```
PS C:\python> ssh -T git@github.com-scarletJeong
Hi scarletJeong! You've successfully authenticated, but GitHub does not provide shell access.

```
---

## Git 리모트 설정

### 1. Git 리모트 URL 설정
1. SSH URL 설정
   ```bash
   git remote set-url origin git@github.com-scarletJeong:scarletJeong/python.git
   ```
2. 설정 확인
   ```bash
   git remote -v
   ```
3. Git Push
   ```bash
   git push -u origin main
   ```
   - 강제 Push
   ```bash
   git push -u secondary main --force
   ```

---

## `config` 파일 예제
```plaintext
Host github.com-repureJiyeon
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes

Host github.com-scarletJeong
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_scarlet
    IdentitiesOnly yes
```

---

## 추가 설정 및 관리

### 1. `config` 파일 권한 확인 및 설정
```bash
icacls "C:\Users\repure\.ssh\config"
icacls "C:\Users\repure\.ssh\config" /grant:r "%USERNAME%:(R)"
```

### 2. `.txt` 확장자 제거
```bash
Rename-Item -Path "C:\Users\repure\.ssh\config.txt" -NewName "config"
ls ~/.ssh
```
### 3. SSH 에이전트 확인
ssh-agent가 실행 중인지 확인하고 SSH 키가 에이전트에 추가되었는지 확인       

- ssh-agent 실행                                 
	`eval $(ssh-agent)`
- SSH 키 추가                                 
	`ssh-add ~/.ssh/id_scarlet`
- SSH 에이전트에 추가된 키 확인                                 
	`ssh-add -l`                                 
	>`id_scarlet` 키가 나와야 함.
### 4. known_hosts 파일 초기화                                
- 기존 `known_hosts` 초기화                                
	`rm ~/.ssh/known_hosts`                                
- 새로 GitHub 키를 추가                                
	`ssh -T git@github.com-scarletJeong`                                


### + 연결된 SSH 키를 제거
`ssh-add -d ~/.ssh/id_rsa`


---

## 깨달은 점
1. **SSH 키 관리의 중요성**
   - SSH 키를 활용하면 안전하고 효율적인 인증 과정 구현 가능.
2. **Git 리모트 설정의 유연성**
   - 여러 SSH 키를 활용해 다양한 리포지토리를 관리 가능.
3. **자동화된 설정 파일 관리**
   - `config` 파일을 활용해 SSH 키와 호스트 간의 연결을 체계적으로 관리 가능.
		


