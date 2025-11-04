# [Coner] 0930스터디_js
> 20240789 이지현 | node.js 2팀 
# 1. npm

## 1. npm 초기 설정
```
$ npm init
```
: create package.json

```
package name: 패키지의 이름
version: 패키지의 버전
description: 프로젝트 설명
entry point: 자바스크립트 실행 파일 진입점
test command: 코드를 테스트할 때 입력할 명령어
git repository:  코드를 저장해둔 깃(Git) 저장소 주소
keywords: npm 공식 홈페이지 - 패키지를 쉽게 찾을 수 있게 함
author: 저자
license: 해당 패키지의 라이선스
```

이후 json 파일이 생성

## 2. npm 패키지 다운로드
```
$ npm install express
```
: express 패키지 설치

### | 개발용 패키지
```
$ npm install --save-dev [패키지] [...]
$ npm i -D
```
: 개발용 패키지 설치

```
$ npm install --save-dev nodemon
``` 
: 소스코드 변경시 자동으로 노드 재실행 패키지
- package.json에 devDependencies 속성이 생기고, 여기에서 개발용 패키지 관리

### | 전역 패키지
```
$ npm install --global rimraf
$ npm i -g [패키지]
```

## 3. node_modules 삭제
```
$ rimraf node_modules

[powershell]
$ npx rimraf node_modules
```

- 프로젝트 폴더내에 package.json과 package-lock.json 밖에 없는 상태가 되었으나, npm install시에 이 json 파일들을 참고해 알아서 설치됨.
- mode_modules는 제외해 커밋

## cf. npx, node_modules 내부 수정하기
- npx : 전역 설치 x 대체제
    ```
    $ npm install --save-dev rimraf
    $ npx rimraf node_modules
    ```
- node_modules 내부 수정하기
    ```
    $ npm i patch-package
    (node_modules 내부의 원하는 패키지 수정하기)
    $ npx patch-package [수정한 패키지 이름]
    ```

## 4. 패키지 버전
```
[메이저 . 마이너 . 패치 ]
```
- 메이저 : 하위호환 x
- 마이너 : 하위호환 가능
- 패치 : 자잘한 수정 사항