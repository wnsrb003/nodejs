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
 
 > var를 사용하면 변수 선언의 경우 할당되는 값이 유동적으로 변경될 수 있는 단점이 있다.\
 > let과 const의 차이점은 변수의 immutable여부이다.\
   let은 변수에 재할당이 가능하지만, const는 변수 재선언, 재할당 모두 불가능하다.

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
