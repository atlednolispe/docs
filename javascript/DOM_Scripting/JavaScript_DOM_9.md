JavaScript DOM 读书笔记 9
========================

## 动画

1. 时间相关的函数

```javascript
variable = setTimeout("fun()", interval);

clearTimeout(variable);
```

2. 移动元素

```javascript
function moveElement(elementID,final_x,final_y,interval) {
    if (!document.getElementById) return false;
    if (!document.getElementById(elementID)) return false;
    var elem = document.getElementById(elementID);
    var xpos = parseInt(elem.style.left);
    var ypos = parseInt(elem.style.top);
    if (xpos == final_x && ypos == final_y) {
        return true;
    }
    if (xpos < final_x) {
        xpos++;
    }
    if (xpos > final_x) {
        xpos--;
    }
    if (ypos < final_y) {
        ypos++;
    }
    if (ypos > final_y) {
        ypos--;
    }
    elem.style.left = xpos + "px";
    elem.style.top = ypos + "px";
    var repeat = "moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")";
    movement = setTimeout(repeat,interval);
}
```

3. 简单动画效果

```javascript
// 创建动画相关元素
function prepareSlideshow() {
    // Make sure the browser understands the DOM methods
    if (!document.getElementsByTagName) return false;
    if (!document.getElementById) return false;
    // Make sure the elements exist
    if (!document.getElementById("linklist")) return false;
    var slideshow = document.createElement("div");
    slideshow.setAttribute("id","slideshow");
    var preview = document.createElement("img");
    preview.setAttribute("src","topics.gif");
    preview.setAttribute("alt","building blocks of web design");
    preview.setAttribute("id","preview");
    slideshow.appendChild(preview);
    var list = document.getElementById("linklist");
    insertAfter(slideshow,list);
    // Get all the links in the list
    var links = list.getElementsByTagName("a");
    // Attach the animation behavior to the mouseover event
    links[0].onmouseover = function() {
        moveElement("preview",-100,0,10);
    }
    links[1].onmouseover = function() {
        moveElement("preview",-200,0,10);
    }
    links[2].onmouseover = function() {
        moveElement("preview",-300,0,10);
    }
}
addLoadEvent(prepareSlideshow);

// 简单的动画效果
function moveElement(elementID,final_x,final_y,interval) {
    if (!document.getElementById) return false;
    if (!document.getElementById(elementID)) return false;
    var elem = document.getElementById(elementID);
    if (elem.movement) {
        clearTimeout(elem.movement);
    }
    if (!elem.style.left) {
        elem.style.left = "0px";
    }
    if (!elem.style.top) {
        elem.style.top = "0px";
    }
    var xpos = parseInt(elem.style.left);
    var ypos = parseInt(elem.style.top);
    if (xpos == final_x && ypos == final_y) {
        return true;
    }
    if (xpos < final_x) {
        var dist = Math.ceil((final_x - xpos)/10);
        xpos = xpos + dist;
    }
    if (xpos > final_x) {
        var dist = Math.ceil((xpos - final_x)/10);
        xpos = xpos - dist;
    }
    if (ypos < final_y) {
        var dist = Math.ceil((final_y - ypos)/10);
        ypos = ypos + dist;
    }
    if (ypos > final_y) {
        var dist = Math.ceil((ypos - final_y)/10);
        ypos = ypos - dist;
    }
    elem.style.left = xpos + "px";
    elem.style.top = ypos + "px";
    var repeat = "moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")";
    elem.movement = setTimeout(repeat,interval);
}
```