# [Coner] 0923스터디_Git
> 20240789 이지현 | node.js 2팀 
# 0. 개요 

### 1) Git과 GitHub
- Git : 코드 변경점 기록
    - 프로젝트의 버전 관리 도구
- GitHub : 온라인 프로젝트 저장소
    - 프로젝트의 공유/백업

## 2) Git 왜 쓰나?
1. 버전 관리
2. 협업
3. 백업

## 3) Git 계정 설정
닉네임 설정 : $`git config --global user.name [닉네임]`

이메일 설정 : $`git config --global user.email [이메일]`

설정 확인 : $`git config -l`

## 4) Git 필수 명령어 리스트
1. `git init`<br>
   : initialize, 초기 세팅/프로젝트 당 한번 씩 입력하면 됨.
   
    *정확한 프로젝트 폴더에서 입력해야함, 경로가 다를 시 `cd` 이용해 경로 변경 후 진행
2. `git add`<br>
   : 저장 전 저장할 파일 지정
   `git add .` -> 현재 경로에서 하위의 모든 변경 파일들 add
2. `git commit`<br>
   : 저장
3. `git status`<br>
   : 저장 여부 확인 명령어
   코드의 변경은 있지만 저장 x -> 붉은 색 글자
4. `git log`<br>
   
5. `git push`<br>
   : 업로드
     `git push origin main`
     * origin == <github 주소>
     * `git branch -M main` : Master -> main로 rename
     * `git push -u origin main` : 이후 git push만 써도 동일 기능
  ---
6.  `git branch`<br>
   : 복제 
   - `git branch [브랜치 이름]`
        : 브랜치 생성
7. `git switch` & `git checkout`

8.  `git merge`
