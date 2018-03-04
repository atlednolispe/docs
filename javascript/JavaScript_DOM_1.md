JavaScript DOM 读书笔记 1
========================

# JavaScript语法

1. <script>标签推荐做法是放在html文档的最后,</body>之前,可以让浏览器更快的加载页面。

```javascript
<!DOCTYPE html>
<html lang="zh-CN">
    <head>
		<meta charset="utf-8" />
		<title>Just a test</title>
	</head>
	<body>
		Mark-up goes here...
		<script src="example.js"></script>
	</body>
</html>
```

2. 数组
JavaScript好像也是一切皆对象。关联对象创建的是对象的属性。

```javascript
// 1.传统数组
var arr = Array("John", "Paul", "George", "Ringo");  // Array(); or Array(4);
var arr = ["John", "Paul", "George", "Ringo"];

// 2.关联数组
// 不推荐使用,应该使用通用的对象Object替代Array
var lennon = Array();
lennon["name"] = "John";
lennon["year"] = 1940;
lennon["living"] = false; 
```

3. 对象

```javascript
var lennon = Array();
lennon.name = "John";
lennon.year = 1940;
lennon.living = false;

// concise
var lennon = {name: "John", year: 1940, living: false};

// key is number
var lennon = {1: "first"}

lennon
{1: "first"}

lennon.1
VM149:1 Uncaught SyntaxError: Unexpected number

lennon."1"
VM151:1 Uncaught SyntaxError: Unexpected string

lennon[1]  // 不能使用.attr的方式获取value,需要用类似数组的方式
"first"
```

4. 操作

```javascript
s = "hello" + 123 // str + num = str
"hello123"

s = "hello" * 3 // 不支持字符串的乘法操作
NaN
```

5. 判断

```javascript
if (condition) {
	statements;
} else {
	statements;
}
```

6. 比较

```javascript
// ===/!==: 比较值 + 类型
// ==/!=: 只比较值
var a = false;
var b = "";

if (a == b) {
	console.log('false == "" is True');
}

if (a !== b) {
	console.log('false !== "" is True');
}

VM260:5 false == "" is True
VM260:9 false !== "" is True

// 逻辑
// && || !
```

7. 循环

```javascript
while (condition) {
	statements;
}

do {
	statements;
} while (condition);  // 这里最后有一个;

for (initial condition; test condition; alter condition) {
	statements;
}

var beatles = Array("John", "Paul", "George", "Ringo");
for (var count=0; count<beatles.length; ++count) {
	console.log(beatles[count]);
}
VM214:3 John
VM214:3 Paul
VM214:3 George
VM214:3 Ringo
```

8. 函数

```javascript
function name(arguments) {
	statements;
}
```

9. 命名

```javascript
// 变量
hello_world

// 函数
sayHi()
```

10. 变量作用域

```javascript
// 函数默认会使用全局变量,内部变量要明确声明为局部变量
function square1(num) {
	total = num * num;
	return total;
}
var total = 50;
var number = square1(20);
console.log(number);
console.log(total);

function square2(num) {
	var total = num * num;
	return total;
}
var number = square2(10);
console.log(number);
console.log(total);

VM220:7 400
VM220:8 400
VM220:15 100
VM220:16 400
```

11. 对象

```javascript
// 对象就是由一些属性和方法组合在一起而构成的一个数据实体

// 创建实例
var jeremy = new Person;
jeremy.age;

// 内建对象
var beatles = new Array();

var num = 7.561;
var num = Math.round(num);
console.log(num);

var current_date = new Date();
var today = current_date.getDay();  // 星期几, getDate()这个月几号
console.log(today);

// 宿主对象(例如浏览器预定义的对象)
// Form, Image, Element, document
```
