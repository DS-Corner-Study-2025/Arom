## **1.1. Node.js 시작하기** 

### **1) Node.js, 런타임(runtime)**

> _Node.js_   
> _**: JavaScript runtime environment** that lets developers create servers, web apps, command line tools and scripts._

-   런타임(runtime) : 특정언어로 만든 프로그램들을 실행할 수 있는 환경
-    노드는 JavaScript Runtime Environment 즉, JavaScript 코드 실행을 가능케하는 환경.

 **왜 Node.js 인가?**

> _Node.js runs the V8 JavaScript engine, the core of Google Chrome, outside of the browser._  
> _This allows Node.js to be very performant._

-   기존의 JavaScript는 웹페이지의 동작을 제어하는 역할. 즉 **브라우저 환경에 종속적**이었음.
-   브라우저 외부의 작업의 속도 ↓ ex) 파일입출력, 네트워크 통신, 데이터베이스 처리
-   Chrome 탑재용으로 개발된, C++ 기반으로 매우 고성능인 JavaScript 구동 엔진(=JavaScript의 해석엔진)인 **V8 등장**
-   Node.js 개발시 V8을 엔진을 채택하며, **브라우저 외부**(서버, 로컬 시스템)에도 **고성능**으로 **JavaScript를 실행**할 수 있게됨.

인용문 전문 / 더 자세한 Node.js의 소개▼ 

_[https://nodejs.org/ko/learn/getting-started/introduction-to-nodejs](https://nodejs.org/ko/learn/getting-started/introduction-to-nodejs)_

---

_\* 편한 이해를 위해 교재와 순서를 다르게 진행_

### **2) 스레드(thread)**

> A Node.js app runs in a single process, without creating a new thread for every request.  
> _노드 앱은 모든 요청에 대해, 새 스레드를 생성하지 않고, 단일 프로세스로 진행된다._



| 프로세스 Process | 스레드 Thread |
| --- | --- |
| 운영체제에서의 할당 작업 단위 | 프로세스 내부의 실행되는 흐름 단위 |
| 자원 공유x | 부모 프로세스의 자원 공유.  |

-   Node.js는 싱글 스레드라고 표현되기도 하나, 엄밀히 따지자면 싱글 스레드는 아님. 
-   실행시 프로세스가 생성 후, 해당 프로세스에서 내부적으로 스레드들을 여러개 생성함.
-   실질적으로 개발자가 직접 제어가능한 스레드(메인 스레드)가 하나이기에 싱글 스레드로 동작한다고 표현되는 것.
-   즉, 개발자가 작성하는 JavaScript 코드는 싱글 스레드 환경처럼 동작함. 

> **그럼 Node.js에서의 멀티 스레드?**   
> \- Tread pool  
> : 노드가 특정 동작 수행(I/O, 파일 등)시 스스로 멀티스레드를 이용  
> \- Worker Thread  
> : 노드 12 에서 안정화된 기능. 많은 CPU작업이 필요할 때, Worker Tread를 사용 할 수 있음.

---

### **3) 논 블로킹(Non-Blocking) , 비동기(Asynchronous)**

-    Node.js에서는 동시에 실행가능한 작업과 동시에 실행 될 수 없는 작업이 존재함.
-   JavaScript에서 돌아가는 것들은 동시에 실행될 수 없으나, 이 외의 I/O작업은 동시에 실행가능.


-   **논블로킹(Non-Blocking)과 블로킹(Blocking)**
    -    논블로킹(non-blocking) 방식은 I/O 작업(파일, 네트워크 등)의 완료를 메인 스레드가 기다리지 않는 것.
    -    즉 차단(Block, 대기)하냐 안하냐의 차이.
    -   ex) I/O 작업이 오래 걸리더라도, 블로킹 방식에서는 해당 작업이 완료될 때까지 전체 프로세스가 대기함.
    -    성능 향상을 위해, 동시 실행 가능한 I/O작업은 최대한 묶어 실행. 
-   **비동기(Asynchronous) 와 동기(Synchronous)**
    -   동기는 요청한 작업에 대해, 완료 여부를 따져 순차대로 처리하는 것.
    -   비동기는 요청한 작업의 완료 여부를 따지지 않음. 여러개에 요청에 있어 응답에서의 순서가 지켜지지 않음.

```js
function longRunningTask() {
  // 오래 걸리는 작업
  console.log('작업 끝');
}

// case 1 - Blocking, Synchronous
console.log('시작');
longRunningTask();
console.log('다음 작업');
// console '시작' -> '작업 끝' -> '다음 작업'

// case 2 - Non Blocking, Asynchronous
console.log('시작');
setTimeout(longRunningTask, 0);
console.log('다음 작업');
// console '시작' -> '다음 작업' -> '작업 끝'
```

> **논블로킹/블로킹 vs 비동기/동기? 어떤 관점으로 바라보냐의 차이.**   
> \- 논블로킹/블로킹은 메인스레드의 동작에 초점  
> \- 비동기/동기는 백그라운드 작업 완료 확인 여부  
>   
> Node.js의 논 블로킹 I/O가 비동기 방식으로 구현(ex. callback func, Promise...)되어 있음

---

### **4) 이벤트 기반( Event-Driven )**

-   특정 이벤트가 발생할 때 무엇을 할지 미리 등록해야함.
-   이벤트 리스너(event listener)에 콜백(callback) 함수를 등록한다고 표현.
-   이벤트 발생 시 이벤트 루프(event loop) 가 해당 콜백을 호출함. 이벤트가 없으면 루프는 대기 상태에 머무름.

-   **호출 스택(Call Stack)** 
    -   함수 호출시 호출 스택에 호출된 함수를 넣음
-    **이벤트 루프(Event Loop)**
    -   이벤트가 동시 발생시, 어떤 이벤트를 먼저 처리할 지 결정.
    -    호출된 콜백 함수를 관리, 실행순서 결정.
    -    노드가 종료될 때까지 이벤트 처리를 위한 작업을 반복
-   **백그라운드(Background)**
    -   setTimeout, 이벤트 리스너들의 대기 영역. 
    -   여러 작업 동시 처리.

-   **태스크 큐 (Task Queue, Callback Queue)**:
    -   이벤트 발생 후, 백그라운드에서는 태스크 큐로 타이머나 이벤트 리스너의 콜백 함수를 보냄

```js
function run() { 
  console.log('3초 후 실행');
}
console.log('시작');
setTimeout(run, 3000);
 
console.log('끝');
```


---

## **1.2. 개발 환경 설정하기**

**1) Node.js 다운로드**

[https://nodejs.org/ko/download/current](https://nodejs.org/ko/download/current)

 [Node.js — Node.js® 다운로드

Node.js® is a free, open-source, cross-platform JavaScript runtime environment that lets developers create servers, web apps, command line tools and scripts.

nodejs.org](https://nodejs.org/ko/download/current)

**2) npm 버전 업데이트**

```
$ npm install -g npm
```

**  
3) Visual Studio Code 설치**

---

## **2.1 알아둬야 할 JavaScript 문법**

### **1) 객체 리터럴**

```js
var sayNode = function() {
  console.log('Node');
};
var es = 'ES';
var oldObject = {
  sayJS: function() {
    console.log('JS');
  },
  sayNode: sayNode,
};
oldObject[es + 6] = 'Fantastic';
oldObject.sayNode(); // Node
oldObject.sayJS(); // JS
console.log(oldObject.ES6); // Fantastic
```

```js
const newObject = {
  sayJS() {
    console.log('JS');
  },
  sayNode,
  [es + 6]: 'Fantastic',
};
newObject.sayNode(); // Node
newObject.sayJS(); // JS
console.log(newObject.ES6); // Fantastic
```

-   콜론(:)과 function 키워드 생략가능
-   속성명과 변수명 동일할 경우 한번만 써도 됨.
-   객체의 속성명을 동적으로 생성

---

### **2) 화살표 함수(Arrow Function)**

```js
function add1(x, y) {
  return x + y;
}


// 화살표 함수 이용
const add2 = (x, y) => {
  return x + y;
  };
const add3 = (x, y) => x + y;
const add4 = (x, y) => (x + y);
```

```js
function not1(x) {
  return !x;
}

// 화살표 함수 사용
const not2 = x => !x;
```

-   function 키워드 대신 => 이용
-   함수 내부에 return문만 존재하는 경우 add3 , add4, not2 처럼 return 생략 가능.

**this 바인딩**

-   기존 function 키워드 이용법에서 this 바인딩과 다름

```js
var relationship1 = {
  name: 'zero',
  friends: ['nero', 'hero', 'xero'],
  // 기존 함수에서의 this 바인딩
  logFriends: function () {
    var that = this; // relationship1을 가리키는 this를 that에 저장
    this.friends.forEach(function (friend) {
      console.log(that.name, friend);
    });
  },
};
relationship1.logFriends();


const relationship2 = {
  name: 'zero',
  friends: ['nero', 'hero', 'xero'],
  // 화살표 함수에서의 this 바인딩
  logFriends() {
    this.friends.forEach(friend => { 
      console.log(this.name, friend);
    });
  },
};
relationship2.logFriends();
```

---

### **3) 구조 분해 할당** ( **Destructuring Assignment )**

-   ( 구조에 맞춰 새로 만들 변수들 ) = ( 구조 분해할 원본 객체, 배열 )

```js
var candyMachine = {
  status: {
    name: 'node',
    count: 5,
  },
  getCandy: function () {
    this.status.count--;
    return this.status.count;
  },
};
var getCandy = candyMachine.getCandy;
var count = candyMachine.status.count;


// 구조 분해 할당 Destructuring 이용
const candyMachine = {
  status: {
    name: 'node',
    count: 5,
  },
  getCandy() {
    this.status.count--;
    return this.status.count;
  },
};
const { getCandy, status: { count } } = candyMachine;
```

-   getCandy(candyMachine.getCandy)를 객체에서 꺼내 getCandy라는 변수에 할당
    -   this가 달라질 수 있으므로 주의
-   status : {count} 값의 복사본을, count에 저장

```js
var array = [‘nodejs’, {}, 10, true];
var node = array[0];
var obj = array[1];
var bool = array[3];

// 배열의 Destructuring
const array = [‘nodejs’, {}, 10, true];
const [node, obj, , bool] = array;
```

---

### **4) 클래스** **( Class )**

-   기존 프로토타입 문법을 클래스 형식으로 표현 가능하도록 함.
-   주의) 내부적으로는 프로토타입으로 동작함.

```js
// Prototype

var Human = function(type) {
  this.type = type || 'human';
};

Human.isHuman = function(human) {
  return human instanceof Human;
}

Human.prototype.breathe = function() {
  alert('h-a-a-a-m');
};

var Zero = function(type, firstName, lastName) {
  Human.apply(this, arguments);
  this.firstName = firstName;
  this.lastName = lastName;
};

Zero.prototype = Object.create(Human.prototype);
Zero.prototype.constructor = Zero; // 상속하는 부분
Zero.prototype.sayName = function() {
  alert(this.firstName + ' ' + this.lastName);
};
var oldZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(oldZero); // true
```

```js
// Class
class Human {
  constructor(type = 'human') {
    this.type = type;
  }

  static isHuman(human) {
    return human instanceof Human;
  }

  breathe() {
    alert('h-a-a-a-m');
  }
}

class Zero extends Human {
  constructor(type, firstName, lastName) {
    super(type);
    this.firstName = firstName;
    this.lastName = lastName;
  }

  sayName() {
    super.breathe();
    alert(`${this.firstName} ${this.lastName}`);
  }
}

const newZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(newZero); // true
```


-   Class로 구조화 되어 직관적, 가독성 향상
-   extends, super 키워드로 상속이 비교적 쉬워짐

---

### **5) 프로미스** **( Promise )** 

-   비동기 처리를 위해 도입되었음. 
-   미래에 어떤 종류의 결과가 반환됨을 약속해주는(promise) 객체 /  실행은 바로하되 결과값은 나중에 받는 객체

```js
const condition = true; // true이면 resolve, false이면 reject

const promise = new Promise((resolve, reject) => {
  if (condition) resolve('성공');
  else reject('실패');
});

// 다른 코드가 들어갈 수 있음
promise
  .then((message) => {console.log(message); }) // 성공(resolve)한 경우 실행  
  .catch((error) => {console.error(error);}) // 실패(reject)한 경우 실행
  .finally(() => { console.log('무조건');}); // 끝나고 무조건 실행
```

> 왜 이런 객체가 필요? callback 와의 차이?  
> 1.1.4) 논블로킹, 비동기 파트에서 언급한 내용의 연장선.  
> I/O를 가져오고 나서, 이 가져온 결과값에 대한 사용이 중요.  
>   
> ex) 피자 주문(토핑선택, 주문) -> 피자 조리(토마토 소스 , 토핑 , 굽기) -> 피자 획득(계산, 획득)  
> call back의 경우  
> \- 각 단계 마다 성공/실패 콜백이 요구됨.   
> \- 많은 중첩, 가독성 ↓, 콜백 지옥  
> \-  유지 보수 어려움  
>   
> promise  
> \- 각 단계의 성공을 .then()을 이용해 처리해 가독성 ↑  
> \- .catch()로 모든 단계에서 발생할 수 있는 모든 에러를 처리 할 수 있음. 안정성 ↑   
> \- 유지 보수 용이

_[https://developer.mozilla.org/ko/docs/Learn\_web\_development/Extensions/Async\_JS/Promises](https://developer.mozilla.org/ko/docs/Learn_web_development/Extensions/Async_JS/Promises)_

```js
//callback 이용
function findAndSaveUser(Users) {
  Users.findOne({}, (err, user) => { // 첫 번째 콜백
    if (err) {
      return console.error(err);
    }
    user.name = 'zero';
    user.save((err) => { // 두 번째 콜백
      if (err) {
        return console.error(err);
      }
      Users.findOne({ gender: 'm' }, (err, user) => { // 세 번째 콜백
        // 생략
      });
    });
  });
}
```

```js
function findAndSaveUser(Users) {
  Users.findOne({})
    .then((user) => {
      user.name = 'zero';
      return user.save();
    })
    .then((user) => {
      return Users.findOne({ gender: 'm' });
    })
    .then((user) => {
      // 생략
    })
    .catch(err => {
      console.error(err);
    });
}
```

**Promise.all(**iterable**)** 

-   프로미스를 한번의 처리 
-   모든 프로미스가 성공시 결과값 반환 / 하나하도 실패시 전체 실패 처리

```
const promise1 = Promise.resolve('성공1');
const promise2 = Promise.resolve('성공2');
Promise.all([promise1, promise2])
  .then((result) => {
    console.log(result); // ['성공1', '성공2'];
  })
  .catch((error) => {
    console.error(error);
  });
```

**Promise.allSettled(**iterable**) - 권장**

-   성공/실패 구분 없이 결과 배열 반환
-   각 프로미스의 결과값 포함 조회(status) 가능

```js
const promise1 = Promise.resolve('성공1');
const promise2 = Promise.reject('실패2');
const promise3 = Promise.resolve('성공3');
Promise.allSettled([promise1, promise2, promise3])
  .then((result) => {
    console.log(result);
/* [
*    { status: 'fulfilled', value: '성공1' },
*    { status: 'rejected', reason: '실패2' },
*    { status: 'fulfilled', value: '성공3' }
*  ]
*/
  })
  .catch((error) => {
    console.error(error);
  });
```

---

### **6) 프로미스의 간결화 async/await**

-   async 함수는 항상 promise를 반환 
    -   then 이나 async함수 내에서 await로 처리 가능
    -   중첩 callback함수를 매우 간결하게 바꿀 수 있
-   await은 Promise의 완료까지 기다린다음 완료 후 결과 값을 반환

_**[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async\_function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)**_

```js
// promise
function findAndSaveUser(Users) {
  Users.findOne({})
    .then((user) => {
      user.name = 'zero';
      return user.save();
    })
    .then((user) => {
      return Users.findOne({ gender: 'm' });
    })
    .then((user) => {
      // 생략
    })
    .catch(err => {
      console.error(err);
    });
}
```

```js
// async/await 문 이용
async function findAndSaveUser(Users) {
  let user = await Users.findOne({});
  user.name = 'zero';
  user = await user.save();
  user = await Users.findOne({ gender: 'm' });
  // 생략
}
```

-   try/catch 문으로 에러 처리 가능

```js
async function findAndSaveUser(Users) {
  try {
    let user = await Users.findOne({});
    user.name = 'zero';
    user = await user.save();
    user = await Users.findOne({ gender: 'm' });
    // 생략
  } catch (error) {
    console.error(error);
  }
}
```

-   for 문 이용 가능

```js
const promise1 = Promise.resolve('성공1');
const promise2 = Promise.resolve('성공2');
(async () => {
  for await (promise of [promise1, promise2]) {
    console.log(promise);
  }
})();
```

---

### **7) Map/Set**

-   **Map**
    -   Key-Value 쌍으로 저장
    -   속성들간 순서 보장
    -   반복문 사용 가능
    -   속성명(key)으로 값 사용 가능
    -   .size() 이용해 속성의 수를 쉽게 알 수 있음 

```js
const m = new Map();

m.set('a', 'b'); // set(키, 값)으로 Map에 속성 추가
m.set(3, 'c'); // 문자열이 아닌 값을 키로 사용 가능합니다
const d = {};
m.set(d, 'e'); // 객체도 됩니다

m.get(d); // get(키)로 속성값 조회
console.log(m.get(d)); // e

m.size; // size로 속성 개수 조회
console.log(m.size) // 3

for (const [k, v] of m) { // 반복문에 바로 넣어 사용 가능합니다
  console.log(k, v); // 'a', 'b', 3, 'c', {}, 'e'
} // 속성 간의 순서도 보장됩니다

m.forEach((v, k) => { // forEach도 사용 가능합니다
  console.log(k, v); // 결과는 위와 동일
});

m.has(d); // has(키)로 속성 존재 여부를 확인합니다
console.log(m.has(d)); // true

m.delete(d); // delete(키)로 속성을 삭제합니다
m.clear(); // clear()로 전부 제거합니다
console.log(m.size); // 0
```

-   **Set**
    -   중복 비허용
    -   배열 유사
    -   반복문 사용 가
    -   다른 언어의 Set과 동일 개념

```js
const s = new Set();
s.add(false); // add(요소)로 Set에 추가합니다
s.add(1);
s.add('1');
s.add(1); // 중복이므로 무시됩니다
s.add(2);

console.log(s.size); // 중복이 제거되어 4

s.has(1); // has(요소)로 요소 존재 여부를 확인합니다
console.log(s.has(1)); // true

for (const a of s) {
  console.log(a); // false 1 '1' 2
}

s.forEach((a) => {
  console.log(a); // false 1 '1' 2
})

s.delete(2); // delete(요소)로 요소를 제거합니다
s.clear(); // clear()로 전부 제거합니다
```

-   Python에서 list -> set -> list로 변환해 중복을 제거하는 것처럼
-   JavaScript의 Set도 Array의 중복을 제거하는 역할로 사용가능

```js
const arr = [1, 3, 2, 7, 2, 6, 3, 5];

const s = new Set(arr);
const result = Array.from(s);
console.log(result); // 1, 3, 2, 7, , 5
```

---

### **8) 널 병합 (nullish coalescing) / 옵셔널 체이닝 (optional chaining)**

**?? 널 병합 연산자 (nullish coalescing)**

-   || 연산자 대용으로 사용
-    falsy 값(0, '', false, NaN, null, undefined) 중 null과 undefined만 따로 구분

```js
const a = 0;
const b = a || 3; // || 연산자는 falsy 값이면 뒤로 넘어감
console.log(b); // 3

const c = 0;
const d = c ?? 3; // ?? 연산자는 null과 undefined일 때만 뒤로 넘어감
console.log(d); // 0;

const e = null;
const f = e ?? 3;
console.log(f); // 3;

const g = undefined;
const h = g ?? 3;
console.log(h); // 3;
```

**?.** **옵셔널 체이닝 연산자 (optional chaining)**

-   null이나 undefined의 속성을 조회하는 경우 에러가 발생하는 것을 방지
-   이때, 아래의 코드에 c?.d와 c?.f(), c?.\[0\]의 값은 undefined

```js
const a = {}
a.b; // a가 객체이므로 문제없음

const c = null;
try {
  c.d;
} catch (e) {
  console.error(e); // TypeError: Cannot read properties of null (reading 'd')
}
c?.d; // 문제없음

try {
  c.f();
} catch (e) {
  console.error(e); // TypeError: Cannot read properties of null (reading 'f')
}
c?.f(); // 문제없음

try {
  c[0];
} catch (e) {
  console.error(e); // TypeError: Cannot read properties of null (reading '0')
}
c?.[0]; // 문제없음
```

---

## **2.2 프론트엔드 JavaScript**

### **1) AJAX (Asynchronous Javascript And XML)**

-   페이지 이동 없이 서버에 요청을 보내고 응답을 받는 기술
-   GET 요청

```js
axios.get('https://www.zerocho.com/api/get')
  .then((result) => {
    console.log(result);
    console.log(result.data); // {}
  })
  .catch((error) => {
    console.error(error);
});
```

```js
(async () => {
  try {
    const result = await axios.get('https://www.zerocho.com/api/get');
    console.log(result);
    console.log(result.data); // {}
  } catch (error) {
    console.error(error);
  }
})();
```

-   POST 요청

```js
(async () => {
  try {
    const result = await axios.post('https://www.zerocho.com/api/post/json', {
      name: 'zerocho',
      birth: 1994,
    });
    console.log(result);
    console.log(result.data);
  } catch (error) {
    console.error(error);
  }
})();
```

---

### **2) FormData**

-   HTML form 태그의 데이터를 동적으로 제어할 수 있는 기능

```js
(async () => {
  try {
    const formData = new FormData();
    formData.append('name', 'zerocho');
    formData.append('birth', 1994);
    const result = await axios.post('https://www.zerocho.com/api/post/formdata', formData);
    console.log(result);
    console.log(result.data);
  } catch (error) {
    console.error(error);
  }
})();
```

---

### **3) encodeURIComponent, decodeURIComponent**

-   AJAX 요청을 보낼 때, ‘http://localhost:4000/search/노드’처럼 주소에 한글이 들어가는 경우
-   한글 주소를 이해하지 못하는 경우를 방지하기 위해 사용.

```js
(async () => {
  try {
    const result = await axios.get(`https://www.zerocho.com/api/search/${encodeURIComponent('노드')}`);
    console.log(result);
    console.log(result.data); // {}
  } catch (error) {
    console.error(error);
  }
})();
```

-   '노드' -encodeURIComponent→ %EB%85%B8%EB%93%9C -decodeURIComponent→ '노드'

### **4) 데이터 속성( data attribute ) 과 dataset**

-   노드를 웹서버로 사용시, 클라이언트와 빈번하게 데이터를 주고 받음
-   프론트엔드에 데이터를 내려보낼때, 보안을 고려함. 
    -   민감한 데이터를 내려보내서는 안됨.
    -   그외의 보안과 관련없는 데이터는 자유롭게 내려보내도 괜찮음
-   이때 보내지는 데이터를 저장하는 방법으로 데이터 속성을 이용
    -   태그의 속성으로 data- 를 넣음. 이것을 데이터 속성이라고 함.
    -   자바스크립트로 쉽게 접근 가능

```js
<ul>
  <li data-id="1" data-user-job="programmer">Zero</li>
  <li data-id="2" data-user-job="designer">Nero</li>
  <li data-id="3" data-user-job="programmer">Hero</li>
  <li data-id="4" data-user-job="ceo">Kero</li>
</ul>
<script>
  console.log(document.querySelector('li').dataset);
  // { id: '1', userJob: 'programmer' }
</script>
```

-   dataset에 데이터를 넣어도 HTML 태그에 반영
    -   ex) dataset.monthSalary = 10000; 넣으면 data-month-salary="10000"이라는 속성 생김.

---

## ♬ QUESTIONS

#### _**빈칸QUIZ**_

1.  Node.js의 비동기 처리 모델은 메인 스레드가 I/O 작업으로 \_\_\_\_\_\_\_\_되지 않도록 함.
2.  이벤트 기반(Event-driven) 구조에서 Node.js는 이벤트 루프(Event Loop) 를 통해 이벤트 발생 시 등록된 \_\_\_\_\_\_ 함수를 호출한다.
3.  비동기 처리 시 콜백 중첩으로 인한 복잡성과 가독성 저하(콜백 지옥)를 해결하기 위해, JS에서는 \_\_\_\_\_\_\_\_\_\_ 객체가 도입되었다.
4.  ES6의 class 문법은 가독성을 높이고, \_\_\_\_\_\_\_\_\_\_ 키워드를 사용하여 상속을 간결하게 표현할 수 있다.
5.  \_\_\_\_\_\_ 은 key-value 쌍으로 저장하며, **속성의 순서를 보장한다.**  \_\_\_\_\_\_ 은 **중복을 허용하지 않으며**, **배열의 중복 제거**에 사용 가능하다.

#### _**코드작성QUIZ**_

#### 1\. 아래 코드를 화살표 함수로 변환하시오.

```js
function multiply(a, b) {
  return a * b;
}
```

2. 아래의 빈칸 1,2에 들어갈 키워드를 작성하시오.

```js
(  1  ) function getUserData(Users) {
  try {
    let user = (  2  ) Users.findOne({});
    user.name = 'Arom';
    user = (  2  ) user.save();
    console.log('저장 완료');
  } catch (error) {
    console.error(error);
  }
}
```

---

  
 

**<빈칸QUIZ 답>**

1.  블로킹(blocking)
2.  콜백(callback)
3.  Promise
4.  extends
5.  Map, Set

   
**<코드작성QUIZ 답>**

1.  ```js
    const multiply = (a, b) => a * b;
    ```
    
2.  (1) async  (2) await

출처 : _조현영,_ _**『**_ _Node.js 교과서 개정 3판__**』**__, 길벗(2022)_

node.js : _[https://nodejs.org/ko/learn](https://nodejs.org/ko/learn)_

   MDN JavaScript : [https://developer.mozilla.org/en-US/](https://developer.mozilla.org/en-US/)

