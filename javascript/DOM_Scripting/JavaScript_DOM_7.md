JavaScript DOM 读书笔记 7
========================

## Cycle to abstract content

1. 建议

```
(1). 最好不要通过js把重要内容添加到网页上

(2). 某个节点后面可能会存在空格或者换行之类的元素节点,不一定以元素节点结尾
```

2. for/in循环

```javascript
for (variable in array) {  // variable是array的index
    statements;
}

3. 获得最后一个元素节点

```javascript
function getLastElement(e) {
    var elements = e.getElementsByTagName('*');
    if (elements.length < 1) return false;
    var last_element = elements[elements.length-1];
    return last_element;
}
```

4. 插入所有abbr的js

```javascript
function displayAbbreviations() {
  if (!document.getElementsByTagName || !document.createElement || !document.createTextNode) return false;
// get all the abbreviations
  var abbreviations = document.getElementsByTagName("abbr");
  if (abbreviations.length < 1) return false;
  var defs = new Array();
// loop through the abbreviations
  for (var i=0; i<abbreviations.length; i++) {
    var current_abbr = abbreviations[i];
    if (current_abbr.childNodes.length < 1) continue;
    var definition = current_abbr.getAttribute("title");
    var key = current_abbr.lastChild.nodeValue;
    defs[key] = definition;
  }
// create the definition list
  var dlist = document.createElement("dl");
// loop through the definitions
  for (key in defs) {
    var definition = defs[key];
// create the definition title
    var dtitle = document.createElement("dt");
    var dtitle_text = document.createTextNode(key);
    dtitle.appendChild(dtitle_text);
// create the definition description
    var ddesc = document.createElement("dd");
    var ddesc_text = document.createTextNode(definition);
    ddesc.appendChild(ddesc_text);
// add them to the definition list
    dlist.appendChild(dtitle);
    dlist.appendChild(ddesc);
  }
  if (dlist.childNodes.length < 1) return false;
// create a headline
  var header = document.createElement("h2");
  var header_text = document.createTextNode("Abbreviations");
  header.appendChild(header_text);
// add the headline to the body
  document.body.appendChild(header);
// add the definition list to the body
  document.body.appendChild(dlist);
}
addLoadEvent(displayAbbreviations);
```

5. 快捷键

```javascript
// accesskey属性可以把一个元素绑定快捷键
// alt + key / alt + shift + key / alt + ctrl + key
<a href="www.mayangbin.com" accesskey="1">
```