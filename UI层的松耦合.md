# UI层的松耦合
## 1. 什么是松耦合
```
1. 做到修改一个组件而不需要更改其他的组件
2. 在一起工作的组件无法达到“无耦合”，组件之间总要共享一些信息
```
## 2. 将JavaScript从CSS中抽离
```
IE8和更早的版本支持CSS表达式，允许在CSS中插入javaScript，会造成性能问题并且造成bug
    > .box{
        width: expression(document.body.offsetWidth + "px");
    }
```
## 3. 将CSS从JavaScript中抽离
```
1. 修改JavaScript的className来修改CSS属性
2. 尽量不要在javaScript中修改CSS属性，如果在相对于页面定位时可以使用
```
## 4. 将JavaScript从HTML分离
```
1. 尽量不要在HTML元素绑定事件
2. 使用事件监听函数
    > function addListener(target, type, handler) {
        if (target.addEventListener) {
            target.addEventListener(type, handler, false);
        } else if (target.attachEvent) {
            target.attactEvent("on" + type, handler);
        } else {
            target["on" + type] = handler;
        }
    }
3. 尽量不要在HTML嵌入JavaScript
```
## 5. 将HTML从JavaScript中抽离
### 从服务器加载
```
1. 将模板置于远程服务器，使用XMLHttpRequest获取
```
### 简单客户端模板
```
1. 使用%s占位符，然后替换
    > <li><a href="%s">%s</a></li>
    function sprintf(text) {
        var i=1, args=arguments;
        return text.replace(/%s/g, function() {
            return (i < args.length) ? args[i++] : "";
        });
    }
    var result = sprintf(templateText, "/item/4", "fourth");
    
2. 注释HTML模板，然后替换
    > <ul id="mylist"><!--<li id="item%s"><a href="%s">%s</a></li>--></ul>
    
    function addItem(url, text) {
        var mylist = document.getElementById("mylist");
        templateText =  mylist.firstChild.nodeValue;
        result = sprintf(template, url ,text);
        div.innerHTML = result;
        mylist.insetAdjacentHTML("beforeend", result);
    }
    addItem("/item/4", "fourth");

3. 放在script标签中，然后替换
    > <script type='text/x-my-template' id="list-item">
        <li><a href="%s">%s</a></li>
      </script>
      var script = document.getElementById("list-item"),
          template = script.text;

```
### 复杂客户端模板
```
使用花括号作为占位符 {{url}}
```
