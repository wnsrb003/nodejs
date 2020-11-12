# nodejs
생활코딩-nodejs

## 객체
> 데이터와 처리방법을 담는 그릇

### Function
> function(함수) 는 변수에 담기기도하고(데이터가 된다) 실행하는 메소드가 되기도 한다.

var p = {\
  v1:'v1',\
  v2:'v2',\
  f1:function(){\
    console.log(this.v1);\
  f2:function(){\
    console.log(this.v2);\
    }\
 }\
 q.f1();\
 q.f2();\
 
 ### var-let-const
 
 > var를 사용하면 변수 선언의 경우 할당되는 값이 유동적으로 변경될 수 있는 단점이 있다.
 > let과 const의 차이점은 변수의 immutable여부이다.
 > let은 변수에 재할당이 가능하지만, const는 변수 재선언, 재할당 모두 불가능하다.

```
// let
let testCase = 'let' // output: let
let testCase = 'let2' // output: Uncaught SyntaxError: Identifier 'testCase' has already been declared
testCase = 'let3' // output: let3
```

```
// const
const testCase = 'const' // output: const
const testCase = 'const2' // output: Uncaught SyntaxError: Identifier 'testCase' has already been declared
testCase = 'const3' // output: Uncaught TypeError:Assignment to constant variable.
```

> 먼저 var는 기본적으로 function scope를 가지게되고, let, const는 block scope를 가지게된다.
```
//var

var foo = "This is String.";
if(typeof foo === 'string'){
	var result = true;
} else {
  var result = false;
}

console.log(result);    // result : true
```

```
//let과 const

var foo = "This is String.";
if(typeof foo === 'string'){
	const result = true;
} else {
  const result = false;
}

console.log(result);    // result : result is not defined
```

### 함수 스코프
> 여러분이 만약 함수 내부에서 변수를 선언한다면, 그 변수는 선언한 함수 내부에서만 사용이 가능합니다.

예제
```
function marcusHello () {
  const hello = 'Hello Marcus!'
  console.log(hello)
}
marcusHello() // 'Hello Marcus!!'
console.log(hello) // Error, hello is not defined
```
### 함수 스코프
> 블록 스코프는 여러분이 중괄호 {}내부에서 const 또는 let으로 변수를 선언하면, 그 변수들은 중괄호 블록 내부에서만 사용이 가능합니다.

예제
```
{
  const hello = 'Hello Marcus!'
  console.log(hello) // 'Hello Marcus!'
}
console.log(hello) // Error, hello is not defined
```

출처
[velog](https://velog.io/@marcus/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8A%A4%EC%BD%94%ED%94%84)

### 동기식 async/await 
```
// 비동기적으로 동작하여 1->3->2 로 출력
// function printNum(number, delaySec){
//   return setTimeout(() => console.log('2'), delaySec);
// }
//
// function logPrintNum(number, delaySec){
//   console.log('1');          //1번쨰
//   printNum(number, delaySec);                          //2번째
//   console.log('3');          //3번째
// }
//
// logPrintNum(1,0);

// 동기적으로 동작 1->2->3 출력
function printNum(number, delaySec){
  return new Promise((resolve) => setTimeout(() =>{
    console.log('2');
    resolve();
  }, delaySec));
}

async function logPrintNum(number, delaySec){
  console.log('1');
  await printNum(number, delaySec);
  console.log('3');
}

logPrintNum(1,0);
```
### async 예외 처리
```
function fetchItems() {
  return new Promise(function(resolve){
    var items = {
      id:12,
      pw:2
    };
        setTimeout(function(){
          resolve(items);
        }, 1000
      );
  });
  }

async function logItems() {
  try{
    var printitems = await fetchItems();
    if(printitems.id == 1){
        console.log(printitems);
    }else {
      console.log('!id : 1');
      throw console.error('ERR!!!!');
    }
  } catch (error){
    console.log(error);
  }
}

logItems();
```
### Middleware
> 미들웨어 함수는 req(요청) 객체, res(응답) 객체, 그리고 어플리케이션 요청-응답 사이클 도중 그 다음의 미들웨어 함수에 대한 엑세스 권한을 갖는 함수이다.\
  미들웨어란 간단하게 말하면 클라이언트에게 요청이 오고 그 요청을 보내기 위해 응답하려는 중간(미들)에 목적에 맞게 처리를 하는, 말하자면 거쳐가는 함수들이라고 보면 되겠다.\
  이 미들웨어들을 어떨 때 사용하면 편리하냐면 페이지를 렌더링할 때 사용자 인증을 앞서 거친 후에 렌더링하고 싶을 때 사용자 인증 미들웨어를 작성하고 앞에 삽입하게 되면 편리하다.
  
> 미들웨어의 특징 간략 정리
1. 모든 코드를 실행
2. 다음 미들웨어 호출(미들웨어가 순차적으로 실행)
3. res, req 객체 변경 가능
4. 요청-응답 주기를 종료(response methods를 이용)

연습코드
```
var express = require('express');
var app = express();
var a = require('./a');
var b = require('./b');
var one = 1;
var two = 2;

var sum = function (req, res, next) {
  req.sum = a.Sum(one, two);
  next();
};

var multi = function (req, res, next) {
  req.multi = b.Multi(one, two);
  next();
};

app.use(sum);
app.use(multi);

app.get('/', function (req, res) {
  var responseText = req.sum + '\n' + req.multi;
  res.send(responseText);
});

app.listen(3000);
```

### express router
- express.Router 클래스를 사용하면 모듈식 마운팅 가능한 핸들러를 작성할 수 있습니다.
- Router 인스턴스는 완전한 미들웨어이자 라우팅 시스템이며, 따라서 “미니 앱(mini-app)”이라고 불리는 경우가 많습니다.
```
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.send('Homepage');
});

router.get('/1ist', function(req,res){
  res.send('main 1ist');
})
module.exports = router;
```

### express 응답 객체
[라우터의 request, response객체](https://luckyyowu.tistory.com/346)

