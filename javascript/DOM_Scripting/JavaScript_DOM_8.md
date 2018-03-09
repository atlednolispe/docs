JavaScript DOM 读书笔记 8
========================

## CSS-DOM

1. 获取元素的样式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    <title>Example</title>
    <script type="text/javascript">
        window.onload = function() {
        var para = document.getElementById("example");
        para.style.color = "black";
        para.style.font = "2em 'Times',serif";
        alert("The font family is " + para.style.fontFamily);
        }
     </script>
</head>
<body>
    <p id="example" style="color: grey; font-family: 'Arial',sans-serif;">
        An example of a paragraph
    </p>
</body>
</html>
```

```
p = document.getElementById('example')

p.style.fontFamily  // camel case instead of "-", if style in name.css, can't be acquired by style attribute
```

2. CSS声明样式的方法

```css
/* 1 */
p {
    font-size: 1em;
}

/* 2 */
.classname {
    font-size: .8em;
}

/* 3 */
#intro {
    font-size: 1.2em;
}

/* 4 */
input[type*="text"] {
    font-size: 1.2em;
}

/**
 *  5
 *  css对位置选择器支持不是很好,不建议使用
 */
p:first-of-type {
    font-size: 2em;
}

3. 获得下一个元素节点

```javascript
function getNextElement(node) {
    if (node.nodeType == 1) {
        return node;
    }
    if (node.nextSibling) {
        return getNextElement(node.nextSibling)
    }
    return null;
}

4. 鼠标悬停响应事件: onmouseover/onmouseout

5. 通过DOM更改元素class属性替代style属性

```javascript
function styleHeaderSiblings() {
    if (!document.getElementsByTagName) return false;
    var headers = document.getElementsByTagName("h1");
    for (var i=0; i<headers.length; i++) {
        var elem = getNextElement(headers[i].nextSibling);
        addClass(elem,"intro");
    }
}
function addClass(element,value) {
    if (!element.className) {
        element.className = value;  // e.className是class属性
    } else {
        element.className += " ";
        element.className += value;
    }
}
function getNextElement(node) {
    if(node.nodeType == 1) {
        return node;
    }
    if (node.nextSibling) {
        return getNextElement(node.nextSibling);
    }
    return null;
}
addLoadEvent(styleHeaderSiblings);
```

6. 建议

```
对函数进行抽象,减少硬编码。
```