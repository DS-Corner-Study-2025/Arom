# 5.1 서비스 운영을 위한 패키지
- localhost -> 실제 서비스 배포 
    - 개발 중에는 서버가 꺼져도 무방하나, 배포 후에는 중단 없이 유지되어야 함.
    - 에러 관리 및 보안 위협 방지를 위한 최소한의 조치 필요.

### 15.1.1 morgan과 express-session
- morgan: 환경 변수인 process.env.NODE_ENV로 배포 환경인지 개발 환경 인지 판단.
    - 배포 : combined 모드 사용 권장
        - 더 많은 로그 정보 기록, 버그 해결 유리
    - 개발 : dev 모드
    ```js
    if (process.env.NODE_ENV === 'production') {
        app.use(morgan('combined'));
    } else {
        app.use(morgan('dev'));
    }
    ```

- express-session 
    - proxy: true, cookie.secure: true (https 적용 시)
    - 보안을 강화
    ```js
    const sessionOption = {
        resave: false,
        saveUninitialized: false,
        secret: process.env.COOKIE_SECRET,
        cookie: {
        httpOnly: true,
        secure: false,
        },
    };
    if (process.env.NODE_ENV === 'production') {
        sessionOption.proxy = true;
        // sessionOption.cookie.secure = true;
    }
    ```

### 15.1.2 시퀄라이즈
- config.json 문제점
    - 비밀번호가 하드코딩
    - JSON 파일 -> 변수 사용 불가

- 해결
    - 설정 파일 변경: config.json을 config.js로 변경하여 변수 사용 가능하게 함 + dotenv 모듈 사용 가능

    - 보안 강화: 비밀번호, 호스트 등 민감 정보는 하드코딩 대신 process.env로 은폐.

    - 배포 환경(production)에서는 logging: false 설정으로 SQL 쿼리 노출 방지.

    - 데이터베이스 비밀번호는 .env 파일에 입력

### 15.1.3 cross-env
- 동적으로 환경 변수 변경, 모든 운영체제에서 동일한 방법으로 환경 변수를 변경 가능하게 하는 패키지

### 15.1.4 sanitize-html, csurf
- XSS(Cross Site Scripting) 
- 사이트에 스크립트를 삽입하는 공격
- 방어
    - 사용자가 게시글 등을 업로드 할 때 스크립트가 포함되어 있는지 검사해, 제거
    - sanitize-html 이용
    ```js
    const sanitizeHtml = require('sanitize-html');
    
    const html = "<script>location.href = 'https://gilbut.co.kr'</script>";
    console.log(sanitizeHtml(html)); 
    ```

- CSRF(Cross Site Request Forgery)
- 사용자가 의도치 않게 공격자가 의도한 행동을 하게 만드는 공격
    - 특정 페이지 방문시 자동으로 로그아웃, 게시글이 써지는 현상
- 방어
    - 사용자가 보낸 요청이 맞는지 식별 가능 하도록 토큰 발행
    - csurf로 토큰을 발급 및 검증하여 사용자가 의도치 않은 위조 요청 차단

### 15.1.5 pm2
- 에러로 서버 종료 시 자동으로 재시작.
- 멀티 프로세싱(Clustering) 지원.
    - 노드는 원래 CPU 코어를 한 개만 사용
    - -i 0 옵션 사용 시 모든 CPU 코어를 활용해 부하 분산.

- 주요 명령어
    - 실행: ``npx pm2 start server.js``
    - 모니터링:` npx pm2 list, npx pm2 monit`
    - 로그: `npx pm2 logs`
    - 재시작: `npx pm2 reload all` (무중단 재시작)
    - 종료 : `npx pm2 kill `

- 프로세스 간 메모리 공유 불가 (세션 문제 발생).
    - 멤캐시드나 레디스 같은 세션 공유 가능하게 하는 서비스 이용

### 15.1.6 winston
- console.log 와 console.error를 대체
    - 휘발성, console객체가 언제 호출 되었는지 파악 어려움
- 로그를 파일이나 데이터 베이스에 저장

- level : 로그 심각도
    - error, warn, info, verbose, debug, silly 심각도에 따라 로그 분류 및 저장 가능
- format : 로그 형식
- transports : 로그 저장 방식
```js
const { createLogger, format, transports } = require('winston');

const logger = createLogger({
  level: 'info',
  format: format.json(),
  transports: [
    new transports.File({ filename: 'combined.log' }),
    new transports.File({ filename: 'error.log', level: 'error' }),
  ],
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new transports.Console({ format: format.simple() }));
}

module.exports = logger;
```

### 15.1.7 helmet, hpp
-  서버의 각종 취약점을 막아주는 미들웨어.

- 배포 환경 필수 적용 권장. 단, 서비스 로직과 충돌하는 옵션(CSP 등)은 공식 문서를 참고해 해제 필요.

### 15.1.8 connect-redis
- 레디스와 익스프레스를 연결하는 패키지
- PM2로 사용 시 세션 공유 불가능 문제 해결
    - 서버메모리에 저장(기존) -> redis라는 외부 데이터베이스에 세션 저장

### 15.1.9 nvm, n
- 노드 버전 업데이트 및 관리 도구.

- OS별 도구 
    -  윈도우 : nvm-installer
    -  리눅스/맥은 n

- 업데이트 후 기존 패키지 오류 시 npm rebuild 실행 또는 node_modules 재설치.


