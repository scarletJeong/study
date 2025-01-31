# GitHub Pull Request 문제 + 해결 

## 개요

GitHub에서 `Create Pull Request` 버튼이 보이지 않는 이유는 `main`과 `master` 브랜치가 완전히 **다른 커밋 히스토리를 가지고 있기 때문**임.

### 오류 메시지

```
There isn’t anything to compare. main and master are entirely different commit histories.
```

즉, 두 브랜치가 공통 조상을 공유하지 않아서 GitHub이 자동으로 비교할 수 없는 상태임.

## 주요 개념 및 구성 요소

### 1. 브랜치 개념

- 브랜치(Branch)는 독립적인 작업을 진행할 수 있도록 분리된 코드 공간임.
- 여러 개발자가 동시에 작업하면서도 메인 코드에 영향을 주지 않게 해줌.
- 새로운 기능 개발, 버그 수정 등을 위해 주로 사용됨.

### 2. `main`과 `master` 브랜치의 차이

- `master`는 Git이 기본으로 제공했던 기본 브랜치였음.
- 최근 GitHub에서는 기본 브랜치를 `main`으로 변경하는 것이 일반적임.
- `main`과 `master` 브랜치가 별개의 커밋 히스토리를 가지면 비교할 수 없음.
- 브랜치 간 관계를 설정해야 Pull Request 생성 가능.

### 3. 해결 방법

#### 방법 1: 로컬에서 `main` 브랜치에 `master` 브랜치 병합 후 푸시

1. **로컬에서 저장소(clone) 가져오기**
   ```sh
   git clone <repository-url>
   cd <repository-name>
   ```
2. **`main`**** 브랜치로 이동**
   ```sh
   git checkout main
   ```
3. **`master`**** 브랜치를 ****`main`****에 병합**
   ```sh
   git merge master --allow-unrelated-histories
   ```
   ⚠️ `--allow-unrelated-histories` 옵션이 중요함. 이 옵션이 없으면 병합이 거부될 수 있음.
4. **충돌(conflict)이 있으면 해결 후 커밋**
   ```sh
   git add .
   git commit -m "Merge master into main"
   ```
5. **GitHub에 ****`main`**** 브랜치 푸시**
   ```sh
   git push origin main
   ```

이제 GitHub에서 `main` 브랜치가 `master` 브랜치의 변경 사항을 반영하게 됨.

#### 방법 2: 강제로 `main` 브랜치를 `master`와 동일하게 만들기

만약 기존 `main` 브랜치 내용을 완전히 덮어도 된다면:

```sh
git checkout master
git branch -D main  # 로컬에서 main 브랜치 삭제
git checkout -b main  # master 기반으로 main 브랜치 새로 생성
git push -f origin main  # 강제 푸시 (주의!)
```

⚠ **이 방법은 기존 ****`main`**** 브랜치의 모든 기록을 삭제하므로 신중하게 사용해야 함.**

## 깨달은 점

- 브랜치 개념을 이해하는 것이 중요함.
- `main`과 `master` 브랜치의 차이를 명확히 알아야 함.
- 브랜치 간 커밋 히스토리가 다르면 Pull Request를 생성할 수 없음.
- `--allow-unrelated-histories` 옵션을 활용하면 병합이 가능하지만, 신중하게 사용해야 함.
- 기존 `main` 브랜치를 덮어쓸 경우, 모든 기록이 사라질 수 있으므로 반드시 백업 필요.

