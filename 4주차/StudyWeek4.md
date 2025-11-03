# [Coner] 0930스터디_js
> 20240789 이지현 | node.js 2팀 
# 1. 노드 기능 알아보기
## 1) module
1) commonJS

2)  ECMAScript , ES 

```js
//var.msj
export const odd = 'MJS 홀수입니다';
export const even = 'MJS 짝수입니다';

//func.mjs
import { odd, even } from './var.mjs';

function checkOddOrEven(num) {
  if (num % 2) { // 홀수이면
    return odd;
  }
  return even;
}

export default checkOddOrEven;


//index.mjs
import { odd, even } from './var.mjs';
import checkNumber from './func.mjs';

function checkStringOddOrEven(str) {
  if (str.length % 2) { // 홀수이면
    return odd;
  }
  return even;
}

console.log(checkNumber(10));
console.log(checkStringOddOrEven('hello'));
```

** js 확장자에서 import를 사용하면 SyntaxError: Cannot use import statement outside a module 에러가 발생 -> package.json에 type: "module" 속성로 변경

|차이점|CommonJs|ECMAScript|
|------|---|---|
|확장자|js,cjs|mjs|
|확장자 생략|가능|불가능|
|인덱스(index) 생략|가능|불가능|
다이내믹 임포트 | 가능 |불가능 
|top level await|불가능|가능|
서로간 호출 | 가능| 가능

**서로 잘 호환되지 않음 혼용 x

3) 다이내믹 임포트
- 조건부로 모듈을 불러오는 것
- ES모듈의 경우 if문 내부에 import 불가능
    ``` js
    const a = true;
    if (a) {
        const m1 = await import('./func.mjs');
        console.log(m1);
        const m2 = await import('./var.mjs');
        console.log(m2);
    }   
    ```

4) __filename, __dirname, (ES)import.meta.url
-  __filename, __dirname이라는 키워드로 경로에 대한 정보를 제공
- 파일에 __filename과 __dirname을 넣어두면 실행 시 현재 파일명과 현재 파일 경로로 바뀜.

## 2) 노드 내장 객체
1) global : window 같은 전역 객체
2) console : 브라우저의 console과 유사.
3) 타이머: setTimeout, setInterval, setImmediate
4) process : 현재 시랭 되는 노드 프로세스 정보

## 3)  노드 내장 모듈
1) os : 운영체제 정보
2) path : 폴더와 파일 경로 조작 도움 모듈
      -운영 체제 별 파일 조작 용이
3) url : 인터넷 주소 조작
4) dns : DNS 다룸
5) crypto : 암호화 모듈
6) util : 각종 편의 기능
7) worker_threads : 멀티 스레드
8) child_process : 다른 프로그램 실행

# 서버 만들기
1) 요청과 응답
   - 클라이언트 -요청(request)→ 서버
   - 서버(요청 읽기, 처리) -응답(response)→

2) localhost(=127.0.0.1)
  - 컴퓨터 내부 주소, 외부 접근 불가
  - `.listen(8080,` : 임의의 포트번호 8080에 노드 서버 연결
  - localhosthttp://localhost:8080 으로 접근
  - 서버 종료시 `Ctrl+C`

3) HTTP 상태 코드
  - 2XX : 성공
  - 3XX : 리다이렉션
  - 4XX : 요청 오류
  - 5XX : 서버 오류, 요청은 정상적으로 왔으나, 서버에 오류. 

4) REST (REpresentational State Transfer)
  - GET : 서버 자원 read
  - POST : 서버에 자원 write
  - PUT : 서버의 자원 치환, 교체
  - PATCH : 서버 자원 일부 수정
  - DELETE : 서버 자원 삭제
  - OPTIONS : 통신 옵션 설명
  *주소 하나가 요청메서드 여러개를 가질 수 있음. 애매한 경우 POST이용
  *REST를 따르는 웹서버 = RESTful

5) 쿠키와 세션
- 사용자 식별 cf) ip/브라우저의 정보는 받아올 수 있음
- key : value 쌍

6) http2/https
- 인증서 정보 - 보안

7) cluster
- 싱글 프로세스로 동작하는 노드가 CPU 코어를 모두 사용할 수 있게 해주는 모듈
- 서버의 무리 줄여줌