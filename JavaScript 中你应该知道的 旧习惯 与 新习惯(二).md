## JavaScript 中你应该知道的 旧习惯 与 新习惯(二)

### 创建对象时使用可计算属性名

旧习惯

- 之前，如果属性名是个变量或者需要动态计算，则只能通过 对象.[变量名] 的方式去访问,且只能运行时才能确定

```js
var name = 'answer';   
var obj = { };
obj[name]=42;
console.log(obj[name]) // 42
```

新习惯

```javascript
var name = 'answer';   
var obj = { 
[name]:42
};
console.log(obj[name]) // 42
```

### 同名变量初始化对象时，使用简写语法

旧习惯：哪怕变量是局部变量，也要写完整key与value

```javascript
function getParams(){
	let name;
	// 其他操作
	return {name:name}
}
```

新习惯：使用简写

```javascript
function getParams(){
	let name;
	// 其他操作
	return {name}
}
```

### 使用 *Object.assign()*代替自定义的扩展方法或者显式复制所有属性

旧习惯 使用遍历key然后复制给新的对象

```javascript
function mergeObjects() {
    var resObj = {};
    for(var i=0; i < arguments.length; i += 1) {
         var obj = arguments[i],
             keys = Object.keys(obj);
         for(var j=0; j < keys.length; j += 1) {
             resObj[keys[j]] = obj[keys[j]];
         }
    }
    return resObj;
}
```

新习惯 使用Object.assign代替

```javascript
const target = { a: 1, b: 2 };

const returnedTarget = Object.assign({}, target);

console.log(returnedTarget); // {a:1,b:2}
```

### 使用属性扩展语法

旧习惯 基于已有对象创建新对象时，使用 Object.assign

```javascript
const target = { a: 1, b: 2 };
const source = { b: 3, c: 4 };

const returnedTarget = Object.assign({},target, source);

console.log(returnedTarget); // {a:1,b:3,c:4}
```

新习惯 使用属性扩展语法

```javascript
const a = { a: 1, b: 2 };
const b = { b: 3, c: 4 };

const returnedTarget = {...a,...b}; // {a: 1, b: 3, c: 4}
```

### 使用 Symbol 避免属性名冲突

旧习惯 使用可读性很差的又很长的字符串来用作属性名来避免冲突

```javascript
const shapeType = {
  triangle: 'Triangle'
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
```

新习惯

```javascript
const shapeType = {
  triangle: Symbol()
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
```

### 使用    *Object.getPrototypeOf/setPrototypeOf*  来代替 *__Proto__*

旧习惯 使用非标准的 __Proto__ 来获取或者设置原型

新习惯 使用标准方法 Object.getPrototypeOf/setPrototypeOf 来获取或者设置原型

具体查看 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/proto

### 使用对象方法的简写语法来定义对象中的方法

旧习惯 key 与 value 的形式

```javascript
const obj = {
    fun : function(){
        // ...
    }
}
```

新习惯

```javascript
const obj = {
    fun(){
        // ...
    }
}
```

### 遍历的姿势-可迭代对象

旧习惯 使用for循环或者 forEach 方法 来循环

```javascript
// for
for(let i = 0; i < array.length; ++i){
    console.log(array[i]);
}
// forEach
array.forEach(entry => console.log(entry));
```

新习惯 当不需要下标的时候，考虑for-of

```javascript
let arr = [1, 2, 3, 4]
for(let item of arr) {
  console.log(item) // 1, 2, 3, 4 
}

需要下标可以考虑
let arr = [1, 2, 3, 4]
for(let [index, value] of arr.entries()) {
  arr[index] = value * 10
}
console.log(arr) // [ 10, 20, 30, 40 ]

```

### 遍历的姿势-DOM

旧习惯 为了遍历DOM集合，将DOM集合转为数组或者使用*Array.prototype.forEach.call* 方法

```javascript
Array.prototype.slice.call(document.querySelectorAll("div")).forEach(
    (div)=>//....
)
    
Array.prototype.forEach.call(document.querySelectorAll("div"),div=>{
    // ...
})
```

新习惯　确保DOM集合是可迭代的，然后使用for-of

```javascript
for(const div of document.querySelectorAll("div"){
    // ...
}
```

### 在使用  Function.prototype.apply() 的大部分场景就可以使用可迭代对象的展开语法

旧习惯

```javascript
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2
```

新习惯　使用可迭代对象的展开语法

```javascript
const numbers = [5, 6, 2, 3, 7];

const max = Math.max([...numbers]);

console.log(max);
// expected output: 7

const min = Math.min([...numbers]);

console.log(min);
// expected output: 2
```



### 该用解构用解构

旧习惯　使用对象中的少量属性就把整个对象保存

```javascript
const bird = getBird();
console.log(bird.age);
console.log(bird.name);
```

新习惯　使用解构赋值，不需要整个对象的时候

```javascript
let {age,name} = getBird();
console.log(age);
console.log(name);
```

### 对可选项使用解构赋值

旧习惯　使用代码处理默认值

```javascript
function check(options){
    let options = Object.assign({},options,{
        a:1,
        b:2,
    })
    if(options.c === undefined){
        options.c = options.a + options.b
    }
    console.log(options.c)
    // ...
}
```

新习惯　将默认值提出来，通过解构来直接处理

```javascript
function check(
{	
    a=1,
    b=2,
    c=a+b
} = {}){
    console.log(c)
    // ...
}

check({a:2}) // 4
```

