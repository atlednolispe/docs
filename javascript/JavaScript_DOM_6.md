JavaScript DOM 读书笔记 6
========================

## Create Tags

1. document.write 不建议使用

```html
<!-- write.html -->
<!DOCTYPE html>
<html lang="zh-CN">
    <head>
		<meta charset="utf-8" />
		<title>Just a test</title>
	</head>
	<body>
		<script src="write.js"></script>
        <script>insert_text("hello");</script>
        <!-- <script src="write.js"></script> 放在实际调用的后面无法产生效果。-->
	</body>
</html>
```

```javascript
// write.js
function insert_text(text) {
	var str = "<p>";
	str += text;
	str += "</p>"
	document.write(str);
}
```

2. element.innerHTML

```javascript
element.innerHTML  // innerHTML属性为str
```

3. 文档中添加元素

```javascript
window.onload = function() {
    var para = document.createElement("p");
    var txt1 = document.createTextNode("I inserted ");
    para.appendChild(txt1);
    var emphasis = document.createElement("em");
    var txt2 = document.createTextNode("this");
    emphasis.appendChild(txt2);
    para.appendChild(emphasis);
    var txt3 = document.createTextNode(" content.");
    para.appendChild(txt3);
    var testdiv = document.getElementById("testdiv");
    testdiv.appendChild(para);
}
```

4. 插入元素

```javascript
parent_element.insertBefore(new_element, terget_element)  // target_element.parentNode = parent_elemen

// 自定义insertAfter
function insertAfter(new_elemen, target_element) {
    var parent = target_element.parentNode;
    if (parent.lastChild == target_element) {
        parent.appendChild(new_element);
    } else {
        parent.insertBefore(new_element, target_element.nextSibling);
    }
}
``` 

5. 初次接触Ajax

```html
<!-- ajax/ajax.html -->
<!DOCTYPE html>
<html lang="zh-CN">
	<head>
		<meta charset="utf-8" />
		<title>Ajax</title>
	<head/>
	<body>
		<div id="new"></div>

		<script src="scripts/addLoadEvent.js"></script>
		<script src="scripts/getHTTPObject.js"></script>
		<script src="scripts/getNewContent.js"></script>
	</body>
</html>
```

```
// ajax/example.txt
This was loaded asynchronously!
```

```javascript
// ajax/addLoadEvent.js
function addLoadEvent(func) {
    var oldOnload = window.onload;
	if (typeof window.onload != 'function') {
		window.onload = func;
	} else {
		window.onload = function() {
			oldOnload();
			func();
		}
	}
}

// ajax/getHTTPObject.js
function getHTTPObject() {
	if (typeof XMLHttpRequest == "undefined")
		XMLHttpRequest = function() {
			try {return new ActiveXObject("Msxml2.XMLHTTP.6.0");}
				catch(e) {}
			try {return new ActiveXObject("Msxml2.XMLHTTP.3.0");}
			    catch(e) {}
			try {return new ActiveXObject("Msxml2.XMLHTTP");}
			    catch(e) {}
			return false;
		}
		return new XMLHttpRequest();
}

// ajax/getNewContent.js
function getNewContent() {
	var request = getHTTPObject();
	if (request) {
		request.open("GET", "example.txt", true);
		request.onreadystatechange = function() {
			if (request.readyState == 4) {
				alert("Response Received.");
				var para = document.createElement("p");
				var txt = document.createTextNode(request.responseText);
				para.appendChild(txt);
				document.getElementById('new').appendChild(para);
			}
		};
		request.send(null);  // 异步执行不会等待send响应
	} else {
		alert('Sorry, your browser doesn\'t support XMLHttpRequest!');
	}
	alert("Function Done.");
}
addLoadEvent(getNewContent);
```

6. 事实

拦截请求,以XMLHttpRequest对象来发送请求。Ajax应用主要依赖于服务器端处理,而非客户端。