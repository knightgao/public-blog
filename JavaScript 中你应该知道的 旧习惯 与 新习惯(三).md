## JavaScript 中你应该知道的 旧习惯 与 新习惯(三)

### 用 async / await 代替 Promise

旧习惯 使用 Promise

```javascript
function fetchJSON(url){
    return fetch(url).then(
    	(response)=>return response.json();
    )
}
```

新习惯 使用 async/await 

```javascript
async function fetchJSON(url){
    const response = await fetch(url);
    return response.json();
}
```

### 使用模板字面量代替字符串连接

旧习惯

```javascript
const getInfo = bird => bird.name + ' ' + bird.age;
```

新习惯

```javascript
const getInfo = bird => `${bird.name} ${bird.age}`;
```

### 使用字符串迭代

旧习惯 使用下标访问字符串中的字符

```javascript
const str = "abcde";
for (let i = 0; i < str.length; ++i){
    // str[i]
}
```

新习惯  视为可迭代对象

```javascript
const str = "abcde";
for (const char of str){
    // char
}
```

### 使用 fing 和 findIndex 方式来代替循环找数组中某个符合条件的

旧习惯 使用for循环 forEach filter取第一项等 

```javascript
let target;
let array = [{id:1},{id:2}];
for (let i = 0; i < array.length; ++i){
    if (array[i].id === 2){
        target = array[i];
        break;
    }
}
```

新习惯 使用 fing 和 findIndex

```javascript
let array = [{id:1},{id:2}];
let target = array.find(i => return i.id === 2);
```

### 使用 Array.fill 代替循环填充数组

旧习惯 使用for循环填充数组

```javascript
// 一维度数组
const len = 5;
const array = new Array(len);
for (let i = 0; i < len; ++i){
    array[i] = 0;
}

// M*N数组
const M = 5;
const N = 5;
const array = new Array(M);
for (let i = 0; i < M; ++i){
    array[i] = new Array(N);
    for (let j = 0; i < N; ++j){
    	array[i][j] = 0;
	}
}
```

新习惯 使用fill

```javascript
// 一维度数组
const len = 5;
const array = new Array(len).fill(0);

// M*N数组
const M = 5;
const N = 5;
// const array = new Array(M).fill(new Array(N).fill(0));
const array = new Array(M).fill(0).map(()=>new Array(N).fill(0)); 
```

### 读取文件时使用 readAsArrayBuffer  取代 readAsBinaryString

旧习惯 使用readAsBinaryString 来处理文件

新习惯 使用readAsArrayBuffer 来处理文件

### 使用Map代替对象

旧习惯 使用对象存储

```javascript
const map = {};
const id = "1";
const obj = {};
map[id] = obj;
```

新习惯 使用Map

```javascript
const store = new Map();
const id = {};
const obj = {};
store.set(id,obj);

// 使用
let target = stroe.get(id);
```

### 使用Set 代替对象做集合

旧习惯 使用对象的key来标记是否存在

```javascript
let birds = {};
birds["xiaohong"] = true;
if(birds["xiaohong"]){
    // 存在
}
```

新习惯 使用Set

```javascript
const birds = new Set();
birds.add("xiaohong");
if(birds.has("xiaohong")){
    // 存在
}
```

### 使用模块代替伪命名空间

旧习惯 导出一整个对象，jquery时代的操作

```javascript
var utils = (function(utils){
    function toString(){
        
    }
    // ...
})(utils || {})
```

新习惯 使用ESM的导入导出操作

```javascript
function toString(){
        
}
const utils = {
    toString:toString
}

export default utils;
export { toString };
```

参考资料：https://es6.ruanyifeng.com/#docs/module-loader

### 将CJS、AMD和其他模块的转为 ESM

旧习惯 打包成CJS AMD 等其他格式，因为没有ESM

新习惯 使用ESM格式，至少新的要支持ESM，拥抱未来

### 使用通用的打包器而不是自研

旧习惯 自研一套打包器

新习惯 拥抱社区，使用通用的有社区支持的打包器

### 使用代理，而不是提供API来get,set 属性

旧习惯 直接提供整个对象，约定不修改

新习惯 改用代理

### 使用代理将部分判断与实现分开

旧习惯 将判断与功能实现写在一起

新习惯 可以考虑将功能函数写对象上，判断或者检测在代理上实现

### 使用二进制字面量

旧习惯 在一些可能更适合二进制的地方，使用十六进制数字

```javascript
const flags = {
    a: 0x01,
    b: 0x02,
    c: 0x04,
    d: 0x08,
}
```

新习惯 在合理的地方采用二进制数字

```javascript
const flags = {
    a: 0b00000001,
    b: 0b00000010,
    c: 0b00000100,
    d: 0b00001000,
}
```

这个多提一句，其实应用挺多的，比如Vue3 里面的 **PatchFlags** 来标记动静节点就是典型的，比如react的车道标记也是

### 使用新的Math函数，而不是各种自己实现的变通方式

旧习惯 使用代码自己实现

新习惯 使用新提供的Math 函数

```javascript
//大部分用不到，这里写几个刷题可能会用到的
** =  Math.pow 
Math.sign(x); // 返回x的符号 【NaN,0,+1,-1】
Math.trunc(x);  // 直接返回整数部分
```

### 使用空值合并

旧习惯  代码判断并赋值

```javascript
const data = res.data === null || res.data === undefined ? 200 : res.data;
```

新习惯 使用空值合并 ??

```javascript
const data = res.data ?? 200;
```

### 使用可选链

旧习惯 先判断是否存在再调用

```javascript
// 属性
let xiaohong = bird && bird.one && bird.one.name;
// 函数
if(xiaohong.talk){
    xiaohong.talk();
}
```

新习惯 使用可选链

```javascript
// 属性
let xiaohong = bird?.one?.name;
// 函数
xiaohong.talk?.();
```

