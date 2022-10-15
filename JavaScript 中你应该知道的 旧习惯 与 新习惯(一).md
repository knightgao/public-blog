## JavaScript 中你应该知道的 旧习惯 与 新习惯(一)

### 使用const 或者 let 替代 var

使用const 声明你不打算改变的值，比如配置，比如对象的引用。

使用let 声明打算改变的值

### 缩小变量的作用域

不用在开头列出所有的变量，现在推荐使用let 或者 const 就近声明，提高代码的可维护性

### 用块状作用域代替匿名函数

````js
for(var n = 0; n < 10; n++){
    (function(value){
        setTimeout(
        	function(){
                console.log(value);
            },10
        );
    })(n)
}
````

使用块状作用率代替匿名函数

```js
for(let n = 0; n < 10; n++){
	setTimeout(function(){
        console.log(n);
    })
}
```

### 使用箭头函数代替各种访问this值的变通方式

为了回调中访问上下文的this，使用了这些 

旧习惯

- 使用变量，var self = this
- 使用 bind
- 在支持的函数中，使用 thisArg 形参

新习惯

使用箭头函数

在不涉及this,或者大多数情况下，箭头函数都可以代替普通函数，更简洁

### 使用参数默认值，而不是代码实现

旧习惯

```javascript
function do(参数){
	if(参数 === undefined){	
		参数 = 默认值
	}
    // XXX
}
```

新的习惯

```javascript
function do(参数 = 默认值){
	// XXX
}
```

### 使用 rest 参数替代 arguments 关键字

旧习惯

```javascript
function func1(a, b, c) {
  console.log(arguments[0]);
  // expected output: 1

  console.log(arguments[1]);
  // expected output: 2

  console.log(arguments[2]);
  // expected output: 3
}

func1(1, 2, 3);
```

新习惯

```javascript
function func1(...rest) {
  console.log(rest[0]);
  // expected output: 1

  console.log(rest[1]);
  // expected output: 2

  console.log(rest[2]);
  // expected output: 3
}

func1(1, 2, 3);
```

### 考虑尾后逗号

旧习惯

```js
const temp = {
	a:1,
	b:2
}
```

新习惯

```js
const temp = {
	a:1,
	b:2,
}
```

好处在哪尼，问题在修改的时候可以少敲一个逗号

### 使用类创建构造函数

旧习惯

```js
// 使用传统的函数语法
var Bird = function Bird(name){
   	this.name = name;
}
Bird.prototype.talk = function talk(string){
    return this.name + 'talk:' + string;
}

var bird = new Bird("小红");

bird.talk("Hi!");
// '小红talk:Hi!
```

新习惯

```javascript
class Bird{
	constructor(name){
        this.name = name;
    }
    talk(string){
        return this.name + 'talk:' + string;
    }
}
var bird = new Bird("小红");

bird.talk("Hi!");
// '小红talk:Hi!
```

