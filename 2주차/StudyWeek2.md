# [Coner] 0930스터디_js
> 20240789 이지현 | node.js 2팀 

### 1.  `console`
   `console.log(message)` : 콘솔에 메세지 표시
-  메세지 표시 - 디버깅
  

\* 객체 ( object - a collection of data and actions )
\* 

### 2. Comment (주석) 
> 주석(Comment) : 코드를 읽는 사람들 위한 메모, 지침, 부연 설명
>  *_프로그램이 실행(run) 될 때, 무시됨._

  1) Single-line Comment
        -  `// 주석 처리 하려는 내용이 한 줄일 때 여기에 적음 `
        -  코드 줄의 바로 뒤에서 사용할 수 있음. 
  <br>
  2) Multi-line Comment 
        - `/* 이 사이에 여러줄의 문장을 입력하면 주석처리됨. 몇줄을 적든 상관이 없음 */`
        - 코드 중간(내부)에서도 `/**/`를 이용해도 문제되지 않음. 
  
### 3. Data Type
1) Primitive Data Type (기본 데이터 유형)
    1) Number : 소수점이 있는 모든 숫자. 
         - ex) `43`, `12.34`,`1514` 
    2) BigInt : 2^53-1보다 큰 모든 수, -(2^53-1)보다 작은 모든 수
    3) String(문자열) : 문자, 숫자, 공백, 기호 등 
         - `'...'` 또는 `"..."`으로 묶어 표현
    4) Boolean : `true` / `false`
    5) Null : 의도적으로 값이 없음을 나타냄.
    6) Undefined : 변수는 선언됨 / 값이 할당되지 않음
         - `null` vs `Undefined`  
          : Undefined는 값 자체가 부여되지 않음, null은 '값이 없는 상태'가 명시적으로 할당.
    7) ~~Symbol~~


2) Object
   : 이후 추가 보충

### 4. Operator(연산자)
1) Arithmetic Operators (산술 연산자)
   - `+`,`-`,`*`,`/`,`%`
     - **`+`는 String Concatenation(문자열 연결)에도 이용
2) Mathematical Assignment Operators
   - `+=`,`-=`,`*=`,`/=`,`%=`
3) The Increment and Decrement Operator
   - `++`,`--`
4) `typeof` Operator
   - variable의 data type을 반환
5) Comparison Operators (비교 연산자)
   - `<`,`>`,`<=`,`>=`,`===`,`!==`
    > <b>[참고] `===(!==)` 와 `==(!=)`의 차이?</b> <br>
    >  전자 -> type까지 비교, 후자 -> 값 비교
    ```
      5 === '5' // false 
      5 == '5'  // true
      5 !== '5' // true
      5 != '5'  // false
    ```
6) Logical Operators (논리 연산자)
   - `||`(or), `&&`(and), `!`(not)
    > <b>[참고] Truthy and Falsy Assignment</b><br>
    > ```let newVar = anotherVar || 'default value'```<br>
    > 변수에 anotherVar가 false일 시 자동으로 dafault value가 할당.

7) Ternary Operator(삼항 연산자)
   - ''

### 5. Variables (변수) 
1) `let`, `var`
   - ES6 방식 
2) `const` constant variable
   - 상수 : 재할당 불가능.
      ```
      TypeError: Assignment to constant variable.
      ```

### 6. Scope
1) Global Scope
    - 전역 변수 스코프
    - 프로그램 파일 전체
2) Block Scope
   - 일반적인 변수 스코프(생명주기) 
   - 블록단위 / 함수 내부 / 반복문 내부 / 조건문 내부
3) Scope Pollution
   - 전역 변수 선언시 global namespace(전역 네임스페이스)로 이동
   - 전역 네임 스페이스에 변수가 너무 많거나 여러 범위에서 변수 재사용하는 경우 발생(=Scope Pollution)
   - 전역 변수와 지역변수가 충돌해 프로그래머의 의도대로 작동하지 않음
   - Scope Pollution 방지 방법
     - global scope 변수 사용 지양
     - 블록 단위로 엄격하게 구분하여 사용하는 것이 가장 바람직


### 7. String (문자열)
<a src = https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String>`[참고] String(문자열) 객체에 대한 문서 `</a>

1) 표기 
     1) `+`
         -  String Concatenation(문자열 연결)
         ```
         console.log("Hello "+"World) 
         // Hello World
          ```
     2) String Interpolation
         - `'...${변수명}...'` 
         - python의 f-sting과 유사
2) Strint Method
   - `toUpperCase()`
     - 전체 문자열 대문자로 변환 후 문자열 반환 
   - `startWith(c)`
      - 문자열의 시작글자가 `c`인지 확인 후 Boolean 반환
   - `trim()`
       - 문자열의 양 끝 부분의 공백 제거
  
### 8. Conditional(조건문)
1) `if`
    ```
    if(조건){ . . 실행문 . .}
    else if(조건){ . . 실행문 . . }
    else{조건 만족 x 시의 실행문}
    ```
2) `switch`
   ```
   switch(expression){
    case 1 : 실행문 break;
    case 2 : . . .
    default : 
   }
   ```

### 9. Loops (반복문)
1) `for` loop
   ```
    for (let i = 0; i < 4; i++) {
    . . .
    }
   ```
     - 배열의 요소 처리에 효과적
      > *[참고]역방향 루프<br>
       > - `i`값을 가장 큰값, 이후 조건설정, i값을 줄여나가도록 설계
2) Nested Loop
   - 반복문 내부에 반복문
3) `while` loop 
   ```
   while(조건문){
    조건문이 true일 동안 반복될 실행문
   }
   ```
   - 무한루프 주의.
4) `do - while` loop
      ```
      do{
        무조건 한번은 실행될 실행문
        이후 조건 확인 후, loop
      }  while(조건문);
      ```
5) `break`
     - 반복문 loop 중단 키워드

### 10. function 
1) Function Declarations (함수 선언)
   ```
   function 함수명( parameter1 , parameter2 ){
    func body
   }
   ```
    - 매개변수 설정시 default값 설정 가능
     - `funtion shoppingList(item1 = 'milk' ){...}`
     - return value 설정 가능
  

    > <b>*[참고] Parameter(매개변수) vs Argument(인자)</b><br>
    > - 매개변수는 함수 정의시 선언되는 변수 ,인자는 함수가 호출될 때 실제로 전달 되는 값

  
    > <b>**[참고] anonymous function (익명 함수)</b><br>
    > - `const func = function(arg){ . . . };` <br>
    > - 함수를 변수처럼 이용 가능 

    > ***[참고] Arrow Functions(화살표 함수)<br>
    > - `const func = (arg)=>{ . . . };` <br>
    > - `function` 키워드 없이 함수 선언 가능 <br>
    > - 매개변수가 하나 -> `()`생략 가능 , 단일 줄 블록 -> `{}`, `return` 생략 가능 (*implicit return )

2) Calling a Function (함수 호출) 
    ```
    function sayHelloMessage(){
      console.log('Hello');
    }

    sayHelloMessage(); // call
    ```
3) Helper Function (헬퍼 함수)
   : 다른 함수 내부에서 쓰이는 보조 역할 함수. 
   - 함수 내부에서 다른 함수를 호출 이때 호출되는 함수를 도우미 함수
   - 코드의 분리, 가독성 향상이 목적

### 11. Array (배열)
<a src =https://www.codecademy.com/resources/docs/javascript/arrays>`[참고] Array(배열)에 대한 문서 `</a>

1) 배열 생성 방법
   - Array Literal 이용
      - `let 배열명 = ['item1','item2',item3'];`
2) Elements
   1) Accessing Elements (배열의 요소 접근)
      - <b>Index</b> 접근, `[]`이용
      - 0번 인덱스 부터 시작
         > *[참고] 배열의 크기보다 큰 인덱스로 접근?<br>
         >-  `undefined` 를 반환함
    2) Update Elements 
      - `arr[i]='새로운 값'` 
         > *[참고] const로 선언한 배열과 let으로 선언한 배열?<br>
         >- `let` 사용 배열 선언 : __배열 자체 재할당 가능(변수 재할당)__, 내부 요소 수정 가능
         >-  `const` 사용 배열 선언 : __변수 재할당 불가능__, 내부 요소 수정 가능.
3) `length` Property
   - String의 length처럼 이용 가능.
   - __배열의 크기__ 반환 
4) Array Method
   1) `push(e1,e2 ..)`
      - 배열의 끝 부분에 요소 추가
      - 초기 배열을 변화 시킴( destructive array method)
   2) `pop()`
      - 삭제할 값 리턴
      - 배열에서 마지막 값을 삭제
      - 초기 배열을 변화 시킴 
   3) `shift()`
      - 맨앞 요소 삭제
   4) `unshift(e1,e2 ..)`
      - 맨 앞에 요소 추가
   5) `slice(sI,eI)`
      - sI(start Index) 부터 eI(end Index)전까지 배열의 요소를 새 배열로 반환
      - 초기 배열 변화 x 
   6) `indexOf('...')`
      - 찾으려는 값을 가진 배열의 인덱스 반환
5) Arrays and Functions (함수 내부에서의 배열)
   - 메소드 이용시 배열이 변화되는 몇가지 사례 존재.
   - 배열 함수 호출 후 변경사항이 그대로 적용 될까? 또는 내부적으로만 변할까?
   - 함수 내부로 배열이 전달 될 시, 변경사항은 외부에서도 적용됨. 
   - 실제로 함수에는 변수가 저장된 메모리 위치에 대한 참조를 전달 = 참조 전달(pass-by-reference) 
    > *[참고]Pass By Reference <br>
    > - 함수를 기준으로 볼때에는 `Call by reference`로 표현 하기도 함
    > - `Pass by Value`, 값의 의한 전달 : __실제 값의 복사본__이 생성되에 함수에 전달. 
6) Nested Arrays (중첩 배열)
     - 배열 내부에 배열
     - `arr[oI][iI]` , `[]`로 차근 차근 접근



### 12. iterators(반복자)
1)  Abstruction (추상화)
    - 복잡한 코드를 재사용, 이해, 디버깅에 용이 하도록 함. 
    - 코드의 모듈화
2) function as Data
     - 매우 긴 함수 이름은 새로운 변수에 할당함으로써 간단하게 호출 가능.
     - 그렇다면 이때 새로운 변수로 역추적은 어떻게 하는가?
       - 함수 객체의 `name` property 이용
3) function as Parameter
     - 함수의 매개변수로 함수를 받을 수 있음. - callback함수를 넘겨줌. 마치 eventlistner 
     - 익명 함수를 인수로 넘겨줄 수 있음
       ```
       const addTwo = num => {
         return num + 2;
       }

       const checkConsistentOutput = (func, val) => {
       const checkA = val + 2;
       const checkB = func(val) ;

       if(checkA === checkB) return func(val);
       else return 'inconsistent results' ;
       }

       console.log(checkConsistentOutput(addTwo,3));
       ```
4) iterator (반복자)
  1) 개요 
   - 반복 특화 js 내장 배열 매서드 ==  iteration methods
   -  <a src =https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#iterative_methods>`[참고] iterator(반복자)에 대한 문서 `</a>
  2) method
     1) `forEach(callbackfunc)`
         - `undefined` 반환  
        ```
        const fruits = ['mango', 'papaya', 'pineapple', 'apple'];

        // Iterate over fruits below
        fruits.forEach(function(fruits){
        console.log('I want to eat a I want to eat a'+fruits)})
        ```

     2) `map()`
        - __새로운 배열__ 반환
        - python과 동일
        ```
        const animals = ['Hen', 'elephant', 'llama', 'leopard', 'ostrich', 'Whale', 'octopus', 'rabbit', 'lion', 'dog'];

        const secretMessage = animals.map(function(animals){return animals[0]})
        console.log(secretMessage.join(''));

        const bigNumbers = [100, 200, 300, 400, 500];
        const smallNumbers = bigNumbers.map(function(bigNumbers){return bigNumbers/100})
        ````
     3) `filter()`
        - 특정 요소 필터링 후 __새로운 배열__ 반환
        - 이때 필터링의 condition이 콜백 함수에서 정의
     4) `findIndex()`
        - 배열에서 첫번째로 만족하는 값의 인덱스를 반환 
     5) `reduce()`
        - 반복하며 배열의 값을 줄임 -> 단 하나의 값을 반환
        ```
        const newNumbers = [1, 3, 5, 7];

        const newSum = newNumbers.reduce(function(accumulator, currentValue){
          console.log('The value of accumulator: ', accumulator);
          console.log('The value of currentValue: ', currentValue);

          return accumulator + currentValue
          }, 10)

        console.log(newSum)
        ```
     6) `some()`, `every()`
        - boolean 타입 반환

### 13. Object (객체)
  - 기본 데이터 타입 이외의 사용자가 객체를 만들 수 있음. 
  - 