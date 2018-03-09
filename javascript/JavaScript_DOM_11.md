JavaScript DOM 读书笔记 11
========================

## SAMPLE

1. CSS整体布局

```css
/**
 *  styles/basic.css
 */
@import url(layout.css);
@import url(color.css);
@import url(typography.css);
```

2. 几种选择器
```css
/* header nav 后代选择器,作为header后代的所有nav元素  .example.pp 类选择器,选择的是class中同时包含example和pp的元素 */
header nav {}

/* tag1, tag2 群组选择器,同样样式作用于多种元素 */
header nav a:link,header nav a:visited {}
```  


3. 页面突出显示

```javascript
function highlightPage() {
    if (!document.getElementsByTagName) return false;
    if (!document.getElementById) return false;  
    var headers = document.getElementsByTagName('header');
    if (headers.length == 0) return false;
    var navs = headers[0].getElementsByTagName('nav');
    if (navs.length == 0) return false;
    
    var links = navs[0].getElementsByTagName("a");
    for (var i=0; i<links.length; i++) {
        var linkurl;
        for (var i=0; i<links.length; i++) {
            linkurl = links[i].getAttribute("href");
            if (window.location.href.indexOf(linkurl) != -1) {  // 获取当前页面的url
            links[i].className = "here";
            var linktext = links[i].lastChild.nodeValue.toLowerCase();
            document.body.setAttribute("id",linktext);
            }
        }
    }
}
```

4. 为了使变量长期存在可以奖变量的值付给一个元素的属性