JavaScript DOM 读书笔记 5
========================

## Images Library v2.0

1. 建议

如果想用JavaScript给某个网页增加行为,不应该让js代码对网页的结构有所依赖,不要预先假设存在某种元素。但要额外增加一层if判断,有更好的做法吗?

2. 多函数绑定onload

```javascript
// 函数个数较少
window.onload = function() {
    firstFunction();
    secondFunction();
}

// 解决方案(Simon Willison)
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

addLoadEvent(firstFunction);
addLoadEvent(secondFunction);
```

3. 三元操作符

```javascript
v = condition ? if true : if false;
```

4. 键盘访问

```
// 最好不要使用onkeypress事件处理函数
onkeypress: 按下键盘上任意键

onclick = tab + enter
```

5. 结构和行为的分离程度越大越好