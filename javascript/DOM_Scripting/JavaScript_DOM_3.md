JavaScript DOM 读书笔记 3
========================

## JavaScript Images Library

1. 通过js改变img的src属性来显示图片

```javascript
function showPic(whichpic) {
    var source = whichpic.getAttribute('href');
    var placeholder = document.getElementById('placeholder');
    placeholder.setAttribute('src', source);
}
```

2. 应用js函数

如果用到多个js文件,为了减少站点的请求次数,应该把js文件合并到一个文件中。

```html
// 事件处理函数: onmouseover, onmouseout, onclick

event = "JavaScript statement(s)"
onclick = "showPic(this);"

<a href="images/fireworks.jpg" title="A fireworks display" onclick="showPic(this); return false;">
// 为a链接添加一个事件处理函数,如果返回true就任务链接被点击了,反之没有。
```

3. 页面加载时调用函数
```javascript
function countBodyChildren() {
	var body_element = document.getElementsByTagName("body")[0];
	alert(body_element.childNodes.length);  // 只显示子节点不包含孙子节点,但包含(元素节点,属性节点,文本节点),两个元素节点中间包含换行的文本节点也会被统计

    // body_element.childNodes[7]; // 属性节点包含在元素节点中 p#description.classp1.classp2
}

window.onload = countBodyChildren; // 页面加载时调用函数,类似回调函数,传递函数对象即可

// element.nodeType 1: 元素节点 2: 属性节点 3: 文本节点
```

4. 改变文本节点的值

<p>元素里面的文本是p元素的子节点。

```javascript
description.firstChild.nodeValue  // description.childNodes[0].nodeValue
```

6. html中引入js和css

```html
<script type="text/javascript" src="scripts/demo.js"></script>
<link rel="stylesheet" href="css/demo.css" media="all">
```