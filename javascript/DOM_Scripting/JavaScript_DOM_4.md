JavaScript DOM 读书笔记 4
========================

## Best Practice

1. 打开新窗口

```javascript
function popUP(URL) {
    window.open(URL, 'new_window', "width=480, height=320");
}
```

多次调用popUP只会在同一个标签页改变网页。

2. 平稳退化

让不支持js的用户也可以正常访问(crawler)

```html
<a href="http://www.mayangbin.com"> onlick="popUp(this.href); return false;">Example</a>
```

3. 分离js内嵌事务处理函数

```
// 在<head>里面嵌入<script src="demo.js"></script>会在body加载之前加载可能会无法显示页面。

// 在</body>之前嵌入<script>书上说文档可能加载不完整,没有完整的DOM,getElementsxxx之类方法不能正常工作,但自己实验了一下好像是可以的。 ??? 

// HTML文档全部加载完毕会触发一个事件,事件有自己的事件处理函数。window对象触发onload事件,document对象已经存在。
```

```html
<!-- onload.html -->
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta http-equiv="content-type" content="text/html; charset=utf-8" />
        <title>Example</title>
    </head>
    <body>
        <a href="http://www.example.com/" class="popup">Example</a>
        <script type="text/JavaScript" src="onload.js"></script>  // script in head is OK.
    </body>
</html>
```

```javascript
// onload.js
window.onload = function() {
    if (!document.getElementsByTagName) return false;
    var lnks = document.getElementsByTagName("a");
    for (var i=0; i<lnks.length; i++) {
        if (lnks[i].getAttribute("class") == "popup") {
            lnks[i].onclick = function() {
                popUp(this.getAttribute("href"));
                return false;
            }
        }
    }
}

function popUp(winURL) {
window.open(winURL, "popup", "width=320,height=480");
}
```

4. 向后兼容

```javascript
// 对象检测
if (!method1 || !method2) return false;

// 浏览器嗅探
// 浏览器可能会报告自己为另一浏览器或者允许用户任意修改信息,正在被更简单健壮的对象检测技术取代
// 对象检测技术是什么???
```

5. 性能

```
// 尽量少访问DOM,查询DOM中的某些元素浏览器都会搜索整个DOM树

// 尽量减少文档的标记数量,不必要的元素会增加DOM树的规模,增加遍历时间

// 多函数做重复事件,得到一组类似元素,可考虑重构代码,保存结果于全局变量

// 合并脚本减少页面加载的请求数量

// head中放置脚本会导致浏览器无法并行夹带其它文件,推荐放置在</body>之前

// 压缩脚本,删除不必要的字节,精简版本:name.min.js,例如Closure Compiler, JSMin, YUI compressor
```