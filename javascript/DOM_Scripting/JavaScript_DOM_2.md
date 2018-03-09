JavaScript DOM 读书笔记 2
========================

## DOM

1. 节点

```
// 元素节点(tag)
<html> <body> <p> <ul>

// 文本节点(总在元素节点的内部): Don't forget to buy this stuff.

// 属性节点: title="a gentle reminder"
<p title="a gentle reminder">Don't forget to buy this stuff.</p>
```

2. CSS

```css
selector {
    property: value;
}

// 节点树上各个元素将继承父元素的样式属性

// class属性
<p class="special">This paragraph has the special class</p>
<h2 class="special">So does this headline</h2>

    // class属性的所有元素
    .special {
        font-style: italic;
    }

    // class属性的特定类型元素
    h2.special {
        text-transform: uppercase;
    }

// id属性
<ul id="purchases">

    // id属性的所有元素
    #purchases {
        border: 1px solid white;
    }

    // id属性的元素里的li元素,不要求是直接的父子关系
    #purchases li {
        font-weight: bold;
    }
```

3. 获取元素

文档中每个元素节点都是一个对象

```javascript
document.getElementById(); // Element

document.getElementsByTagName();  // Elements
document.getElementsByTagName('*');  // 可以使用通配符匹配所有元素

document.getElementsByClassName();
document.getElementsByClassName("important sale"); // multiply class

function getElementsByClassName(node, classname) {
    if (node.getElementsByClassName) {
        return node.getElementsByClassName(classname);  // browser's default function
    } else {
        var results = new Array();
        var elems = node.getElementsByTagName('*');
        for (var i=0; i<elems.length; i++) {
            if (elems[i].className.indexOf(classname) != -1) {
                results[results.length] = elems[i];
            }
        }
        return results;
    }
}
```

4. 判断对象类型

```javascript
var v = ...
typeof v;

```

5. 获取和设置属性

```javascript
object.getAttribute(attribute)  // object要求为一个元素节点对象

object.setAttribute(attribute, value)  // object要求为一个元素节点对象

// setAttribute后,浏览器view source不显示属性值的变化,inspect可以看到属性变化
```