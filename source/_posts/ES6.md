# ES6

# 1、简介

ES6， 全称 **ECMAScript 6.0** ，是 JavaScript 的下一个版本标准，2015.06 发版。

ES6 主要是为了解决 ES5 的先天不足，比如 JavaScript 里并没有类的概念，但是目前浏览器的 JavaScript 是 ES5 版本，大多数高版本的浏览器也支持 ES6，不过只实现了 ES6 的部分特性和功能。

目前各大浏览器基本上都支持 ES6 的新特性，其中 Chrome 和 Firefox 浏览器对 ES6 新特性最友好，IE7~11 基本不支持 ES6。

# 2、语法学习

## 1、let和const

let 声明的变量只在 let 命令所在的代码块内有效。

```javascript
{
    let a = 0;
    a    //0
}
a // 报错 ReferenceError: a is not defined
```

> - 代码块内有效
> - 不能重复声明
> - 适合用于循环计数器
> - 不存在变量提升

const 声明一个只读的常量，一旦声明，常量的值就不能改变。

```javascript
const PI = "3.1415926";
PI  // 3.1415926

const MY_AGE;  // SyntaxError: Missing initializer in const declaration    
```

> const 保证的不是变量的值不变，而是保证变量指向的内存地址所保存的数据不允许改动。

## 2、解构赋值

### 2.1、概述

概念：解构赋值是对赋值运算符的扩展。针对数组或者对象进行模式匹配，然后对其中的变量进行赋值。

作用：代码书写上简洁易读，语义更加清晰明了，也方便了在复杂对象中数据字段的获取。

组成：解构的源，解构赋值表达式的右边部分；解构的目标，解构赋值表达式的左边部分

### 2.2、数组模型的解构

**基本用法**

```javascript
let [a,b,c] = [1,2,3];
// a = 1
// b = 2
// c = 3
```

**嵌套**

```javascript
let [a,[[b],c]] = [1,[[2],3]];
// a = 1
// b = 2
// c = 3
```

**忽略**

```javascript
let [a,,b] = [1,2,3];
// a = 1
// c = 3
```

**不完全解构**

```javascript
let [a = 1,b] = []; // a = 1, b = undefined
```

**剩余运算符**

```javascript
let [a,...b] = [1,2,3];
// a = 1
// b = [2,3]
```

**字符串**

在数组结构中，解构目标若为可遍历对象，皆可进行解构赋值

```javascript
let = [a,b,c,d,e] = 'hello';
// a = 'h'
// b = 'e'
// c = 'l'
// d = 'l'
// e = 'o'
```

**解构默认值**

```javascript
let = [a = 2] = [undefined];  //a = 2
```

当解构模式有匹配结果，且匹配结果是undefined时，会触发默认值作为返回结果

```javascript
let [a = 3, b = a] = [];
let [a = 3, b = a] = [1];
let [a = 3, b = a] = [1, 2];
// 以上三个表达式中,a,b的值是多少
```

### 2.3、对象模型解构

**基本用法**

```javascript
let {name,age} = {name:'tom',age:4}
// name = 'tom'
// age = 4

let {name:Name} = {name:'jerry'}
// Name = 'jerry'
```

**嵌套、忽略**

```javascript
let obj = {p:['hello',{y:'world'}]};
let {p:[x,{y}]} = obj;
// x = 'hello'
// y = 'world'

let obj = {p:['hello',{y:'world'}]};
let {p:[x,{}]} = obj;   // x = hello
let {p:[,{y}]} = obj;   // y = world
```

**不完全解构**

```javascript
let obj = {p:[{y:'world'}]};
let {p:[{y},x]} = obj;
// x = undefined
// y = 'world'
```

**剩余运算符**

```javascript
let = {a, b, ...rest} = {a:10, b:20, c:30, d:40};
// a = 10
// b = 20
// rest = {c:30,d:40}
```

**解构默认值**

```javascript
let {a = 10, b = 5} = {a:3};
// a = 3; b = 5;
let {a: aa = 10, b: bb = 5} = {a:3};
// aa = 3; bb = 5; 
```

## 3、Symbol

### 3.1、概述

数据类型除了原先的Number、String、Boolean、Object、null、undefined，新增了Symbol

ES6中引入了一种新的原始数据类型Symbol，表示独一无二的值，用来定义对象的唯一属性名

### 3.2、基本用法

Symbol 函数栈不能用 new 命令，因为 Symbol 是原始数据类型，不是对象。可以接受一个字符串作为参数，为新创建的 Symbol 提供描述，用来显示在控制台或者作为字符串的时候使用，便于区分。

```javascript
let sy = Symbol("KK");
console.log(sy);  // Symbol(kk)
typeof(sy);       // "symbol"

// 相同参数Symbol()返回的值不相等
let sy1 = Symbol("KK")
sy === sy1;     //false
```

### 3.3、使用场景

**作为属性名**

由于每一个Symbol的值都是不相等的，所以Symbol作为对象的属性名，可以保证属性不重名

```javascript
let sy = Symbol("key1");

//写法1
let syObject = {};
syObject[sy] = "kk";
console.log(syObject);     // {Symbol(key1):"kk"}

//写法2
let syObject = {
    [sy]:"kk"
};
console.log(syObject);    // {Symbol(key1):"kk"}

//写法3
let syObject = {};
Object.defineProperty(syObject, sy, {value: "kk"});
console.log(syObject);   // {Symbol(key1): "kk"}
```

通过属性名获取属性值

```javascript
let sy = Symbol("key1");
syObject[sy] = "kk";

syObject[sy];   // "KK"
syObject.sy;    // undefined
```

> Symbol值作为属性名时，该属性是公有属性不是私有属性，可以在类的外部访问。但是不会出现在for...in、for...of的循环中，也不会被Object.keys()、Object.getOwnPropertyNames()返回。如果要读取一个对象的Symbol属性，可以通过Object.getOwnPropertySymbols()和Reflect.ownkerys()取到

```javascript
let syObject = {};
syObject[sy] = "kk";
console.log(syObject);

for(let i in syObject){
    console.log(i);
    //无输出
}

Object.keys(syObject);                    //[]
Object.getOwnPropertySymbol(syObject);    //[Symbol(key1)]
Reflect.ownkeys();                        //[Symbol(key1)]
```

**Symbol.for()**

Symbol.for() 类似单例模式，首先会在全局搜索被登记的 Symbol 中是否有该字符串参数作为名称的 Symbol 值，如果有即返回该 Symbol 值，若没有则新建并返回一个以该字符串参数为名称的 Symbol 值，并登记在全局环境中供搜索。

```javascript
let yellow = Symbol('Yellow');
let yellow = Symbol.for("Yellow");
```

## 4、Map与Set

### 4.1、Map对象

#### 4.1.1、Maps和Object的区别

>- 一个Object的键只能是字符串或者是Symbols，但一个Map的键可以使任意值
>- Map中的键值是有序的（FIFO原则），而添加到对象中的键则不是
>- Map的键值对个数可以从size属性获取，而Object的键值对个人只能手动计算
>- Object都有自己的原型，原型链上的键名有可能和自己在对象上设置的键名产生冲突

#### 4.1.2、Map中的key

```javascript
var myMap = new Map();

// key是字符串
var keyString = "a string";
myMap.set(keyString,"和键'a string'关联的值");

myMap.get(keyString);    //"和键'a string'关联的值"
myMap.get("a string");   //"和键'a string'关联的值"

// key是对象
var keyObj = {};
myMap.set()
```

5、Reflect和Proxy

6、字符串

7、数值

8、对象

9、数组

10、函数

11、迭代器

12、Class类

13、模块

14、Promise对象

15、Generator函数

16、async函数

