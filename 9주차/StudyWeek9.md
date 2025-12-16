
**1. API 서버 이해하기**
- API (Application Programming Interface): 다른 애플리케이션에서 현재 프로그램의 기능을 사용할 수 있게 허용하는 접점.
- 웹 API: API를 서버에 올려 URL을 통해 접근할 수 있게 만든 창구. 인증된 사용자에게 제공하고 싶은 정보만 공개 가능함.
- 크롤링(Crawling)과의 차이:
    - 크롤링: 웹사이트가 제공하지 않는 정보를 자체적으로 수집. 트래픽 증가 유발 및 법적 문제 소지 있음.
    - API: 공식적으로 허용된 제한 내에서 정보를 제공하므로 안정적임.
- 활용: 프런트엔드와 분리된 서버 운영, 모바일 앱 서버, 다른 서비스와의 데이터 공유 등.

**2. 프로젝트 구조 갖추기**
- 구조 분리:
    - nodebird-api (Provider): API 제공자. NodeBird의 DB와 모델을 공유하며 데이터를 JSON 형식으로 응답함.
    - nodecat (Consumer): API 사용자. API를 호출하여 데이터를 가져와서 사용하는 클라이언트 서비스.
- 패키지: uuid (범용 고유 식별자) 패키지 추가 (API 키 발급용).
도메인 모델 (Domain): API 사용자를 등록하고 인증하기 위한 테이블.
- host: API를 사용할 클라이언트의 도메인 주소.
- type: 요금제 종류 (free, premium).
clientSecret: API 인증에 사용될 비밀 키 (UUID v4 버전 사용).

**3. JWT 토큰으로 인증하기**

- JWT (JSON Web Token): JSON 데이터를 저장하는 토큰.
- 구성: 헤더(Header) . 페이로드(Payload) . 시그니처(Signature)
    - 헤더 (Header): 토큰 종류와 해시 알고리즘 정보.
    - 페이로드 (Payload): 토큰에 담긴 실제 데이터(사용자 ID, 닉네임 등). 노출되어도 무방한 정보만 담아야 함. 디코딩이 쉬우므로 민감 정보(비밀번호 등) 절대 포함 금지.
    - 시그니처 (Signature):  비밀 키를 이용해 토큰의 변조 여부를 확인.
- 장점: 내용물(Payload)을 믿고 사용할 수 있어 DB 조회 비용 절감 (Stateless).
- 단점: 랜덤 문자열 방식보다 용량이 큼.
- 구현: jsonwebtoken 모듈 사용.
- 발급: jwt.sign (내용, 비밀키, 옵션).
- 검증: jwt.verify (토큰, 비밀키).
- 응답 코드:
    - 419 (커스텀): 토큰 유효 기간 만료.
    - 401: 유효하지 않은 토큰(변조 등).
- 에러 처리:
    - 401: 토큰 위조, 시그니처 불일치.
    - 419 (커스텀): 토큰 유효 기간 만료.


**4.  다른 서비스에서 호출하기**
- API 사용 흐름 (NodeCat):
요청 시 세션에 토큰이 있는지 확인.
- 토큰 없음: clientSecret을 본문에 담아 API 서버에 토큰 발급 요청(POST /token). 발급받은 토큰을 세션에 저장.
- 토큰 있음: authorization 헤더에 토큰을 넣어 API 요청.
- 테스트: GET /test 라우터를 통해 토큰 유효성 검증 및 페이로드 내용 확인.

**5. SNS API 서버 만들기**

- 라우터 버전 관리: API 수정 시 기존 사용자에게 영향을 주지 않기 위해 URL에 버전(v1)을 명시함.
- 기능 구현:
    - GET /posts/my: 자신이 올린 게시글 조회.
    - GET /posts/hashtag/:title: 해시태그로 게시글 검색.
- 응답 형식 통일: JSON 응답 시 code(HTTP 상태 코드 또는 커스텀), message(에러 메시지), token 또는 payload(데이터) 등으로 형식을 갖춰 클라이언트의 처리를 도움.


**6. 사용량 제한 구현하기**

- 목적: 과도한 API 호출(DDoS 등) 방지 및 등급별(유료/무료) 서비스 차별화.
- 도구: express-rate-limit 패키지.
    - windowMs: 기준 시간 (예: 1분).
    - max: 허용 횟수.
- handler: 제한 초과 시 응답 로직.
- 응답 코드: 제한 초과 시 429 (Too Many Requests) 반환.
- 버전 업그레이드:
    - v2 라우터 생성 (유효기간 증가, 사용량 제한 적용).
    - v1 라우터에 deprecated 미들웨어 적용 -> 410 (Gone) 에러와 경고 메시지 응답.
- 한계: 서버 재시작 시 메모리 초기화됨 (실무에선 Redis 등 DB 연동 필요).


**7. CORS 이해하기**

- CORS (Cross-Origin Resource Sharing): 브라우저가 보안을 위해 다른 도메인(프로토콜, 포트 포함) 간의 요청을 차단하는 정책.
    - 서버-서버 간 통신(NodeCat 서버 -> NodeBird API)은 문제없음.
    - 브라우저(NodeCat 프런트) -> NodeBird API 요청 시 발생.
- 동작: 실제 요청 전 OPTIONS 메서드로 예비 요청을 보내 허용 여부 확인.
- 해결: cors 패키지 사용 (Access-Control-Allow-Origin 헤더 설정).
- 보안 이슈: 프런트엔드 코드에 clientSecret 노출됨.
- 대응:
    - 모든 도메인 허용 대신, origin 헤더(요청 출처)와 DB에 등록된 
    - 도메인(host)이 일치할 때만 CORS를 허용하는 커스텀 미들웨어 작성.
    - credentials: true 옵션으로 쿠키 공유 허용.
- 대안: 프록시 서버 사용 - 브라우저와 같은 도메인의 서버가 대신 요청

**8. 프로젝트 마무리하기**
- 보안 강화: 클라이언트용 키(공개)와 서버용 키(비밀)를 구분하여 발급/관리 권장.
- 문서화: Swagger, apidoc 등을 활용해 API 사용 설명서(요청/응답 형식 등) 작성 필요.

**9. 핵심 실습 코드**
1) nodebird-api

```js
//models/domain.js
class Domain extends Sequelize.Model {
static initiate(sequelize) {
    Domain.init({
            host: {
                type: Sequelize.STRING(80),
                allowNull: false,
            },
            type: {
                type: Sequelize.ENUM('free', 'premium'),
                allowNull: false,
            },
            // clientSecret은 UUID 타입으로 설정
            clientSecret: {
                type: Sequelize.UUID,
                allowNull: false,
            },
        }, {
            sequelize,
            timestamps: true,
            paranoid: true,
            modelName: 'Domain',
            tableName: 'domains',
        });	
    }

    static associate(db) {
    	db.Domain.belongsTo(db.User);
    }
};

module.exports = Domain;
```

```js
//middlewares/index.js

const jwt = require('jsonwebtoken');
const rateLimit = require('express-rate-limit');
const cors = require('cors');

// 1. JWT 검증
exports.verifyToken = (req, res, next) => {
  try {
    // 요청 헤더의 토큰 검증
    res.locals.decoded = jwt.verify(req.headers.authorization, process.env.JWT_SECRET);
    return next();
  } catch (error) {
    if (error.name === 'TokenExpiredError') { //유효 기간 초과
      // 유효 기간 만료 시 419 코드 반환
      return res.status(419).json({ code: 419, message: '토큰이 만료되었습니다' });
    }
    return res.status(401).json({ code: 401, message: '유효하지 않은 토큰입니다' });
  }
};

// 2. 사용량 제한
exports.apiLimiter = rateLimit({
  windowMs: 60 * 1000, // 1분
  max: 10, // 허용 횟수
  handler(req, res) {
    // 제한 초과 시 429 코드 반환
    res.status(this.statusCode).json({
      code: this.statusCode,
      message: '1분에 열 번만 요청할 수 있습니다.',
    });
  },
});

// 3. CORS
exports.corsWhenDomainMatches = async (req, res, next) => {
  const domain = await Domain.findOne({
    where: { host: new URL(req.get('origin')).host },
  });
  if (domain) {
    cors({
      origin: req.get('origin'),
      credentials: true,
    })(req, res, next);
  } else {
    next();
  }
};
//nodebird-api/routes/v1.js

const express = require('express');
const { renderLogin, createDomain } = require('../controllers');
const { isLoggedIn } = require('../middlewares');

const router = express.Router();

// POST /v1/token
router.post('/token', createToken);

// POST /v1/test
router.get('/test', verifyToken, tokenTest);

module.exports = router;
// //nodebird-api/routes/v2.js

const express = require('express');

const { verifyToken, apiLimiter } = require('../middlewares');
const { createToken, tokenTest, getMyPosts, getPostsByHashtag } = require('../controllers/v2');

const router = express.Router();

router.use(corsWhenDomainMatches); // CORS

// POST /v2/token
router.post('/token', apiLimiter, createToken);

// POST /v2/test
router.get('/test', apiLimiter, verifyToken, tokenTest);

// GET /v2/posts/my
router.get('/posts/my', apiLimiter, verifyToken, getMyPosts);

// GET /v2/posts/hashtag/:title
router.get('/posts/hashtag/:title', apiLimiter, verifyToken, getPostsByHashtag);

module.exports = router;
```



2) nodecat
```js
// controllers/index.js

const axios = require('axios');

const URL = process.env.API_URL;
axios.defaults.headers.origin = process.env.ORIGIN; // origin 헤더 추가

const request = async (req, api) => {
  try {
    if (!req.session.jwt) { // 세션에 토큰이 없으면
      const tokenResult = await axios.post(`${URL}/token`, {
        clientSecret: process.env.CLIENT_SECRET,
      });
      req.session.jwt = tokenResult.data.token; // 세션에 토큰 저장
    }
    return await axios.get(`${URL}${api}`, {
      headers: { authorization: req.session.jwt },
    }); // API 요청
  } catch (error) {
    if (error.response?.status === 419) { // 토큰 만료 시 토큰 재발급받기
      delete req.session.jwt;
      return request(req, api);
    } // 419 외의 다른 에러이면
    throw error;
  }
};
 
exports.getMyPosts = async (req, res, next) => {
  try {
    const result = await request(req, '/posts/my');
    res.json(result.data);
  } catch (error) {
    console.error(error);
    next(error);
  }
};
 
exports.searchByHashtag = async (req, res, next) => {
  try {
    const result = await request(
      req, `/posts/hashtag/${encodeURIComponent(req.params.hashtag)}`,
    );
    res.json(result.data);
  } catch (error) {
    if (error.code) {
      console.error(error);
      next(error);
    }
  }
};

exports.renderMain = (req, res) => {
  res.render('main', { key: process.env.CLIENT_SECRET });
};
```

