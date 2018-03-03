# JavaScript DOM 读书笔记 1
========================

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

4. 判断

```javascript
if (condition) {
	statements;
} else {
	statements;
}

// 比较
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

5. 比较

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