[slide]
## DOM事件
## 王明欢
## 2017-04-19

[slide]

## 一、什么是事件
javaScript 与 HTML 之间的交互是通过事件实现的。事件，就是文档或浏览器窗口中发生的一些特定的交互瞬间。可以使用侦听器（或处理程序）来预订事件，以便事件发生时执行相应的代码。---- js高级程序设计

[slide]

## 二、事件流

[slide]
 
- IE: 事件冒泡 （event bubbling）   
- Netscape： 事件捕获（event capturing） 

- DOM2级事件规定的事件流   

![DOM2-事件流](../img/01.png)

[slide]

## 三、事件处理程序

[slide]
## 3.1 事件绑定-DOM中直接绑定 
- 例子
````
	<a onclick="clickLog('from=es_recommend_shouyeyy_right')" href="/jiadian/">高性价比家电</a>
````
- 缺点：耦合度高，跨浏览器兼容性差

[slide]

## 3.2 事件绑定-js中绑定
- 例子
````
	var btn = document.getElementById("myBtn");
	btn.onclick = function(){
		alert("Clicked");
	};
````
- 缺点：前后覆盖
[slide]

## 3.3 事件监听

- `addEventListener()`  

````
	var btn = document.getElementById("myBtn");
	btn.addEventListener("click",function(){
       alert("Clicked");
	},false)  
    // false(冒泡)，true代表事件捕获
    //Chrome、FireFox、Opera、Safari、IE9.0及其以上都支持
   
````
- `attachEvent` 

````
	var btn = document.getElementById("myBtn");
	btn.attachEvent("onclick",function(){
       alert("Clicked");
	})  
   //IE8.0及其以下版本支持
````
[slide] 

## 事件监听的取消  

- `addEventListener()` <---->`removeEventListener()` 

- `attachEvent` <----> `detachEvent`  
-  匿名回调函数无法解绑, 必须要回调函数的引用
 ````
   btn.addEventListener("click",function(){
       alert("Clicked");
	},false) 
	btn.removeEventListener("click",function(){
       alert("Clicked");
	},false) 

 ````  
[slide]
兼容写法：  
```` 
var EventUtil = {
    addHandler: function(element, type, handler) {
        if (element.addEventListener) {
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent) {
            element.attachEvent("on" + type, handler);
        } else {
            element["on" + type] = handler;
        }
    },
    removeHandler: function(element, type, handler) {
        if (element.removeEventListener) {
            element.removeEventListener(type, handler, false);
        } else if (element.detachEvent) {
            element.detachEvent("on" + type, handler);
        } else {
            element["on" + type] = null;
        }
    }
};
```` 

[slide]
## Event对象
- 事件第一次触发的时候被创建
- 作为第一个参数传递给事件监听的回调函数(非IE)
- IE下通过 window.event访问
- click事件的event
![click的event对象](../img/03.png)

[slide]
## Event对象下的方法-阻止冒泡  

- 非IE: `event.stopPropagation()/event.stopImmediatePropagation()`
- IE: `window.cancelBubble = true` 

```` 
	var btn = document.getElementById("btn");
	var wrap= document.getElementById("wrap");
	 btn.addEventListener("click",function(event){
	    //event.stopPropagation();
		//event.stopImmediatePropagation();
	       alert("我是监听函数1")
	    },false) 
	 btn.addEventListener("click",function(event){
	       alert("我是监听函数2")
	    },false) 
	 wrap.addEventListener("click",function(event){
	       alert("我是父级监听函数")
	    },false) 
```` 

<iframe data-src="https://wangminghuan.github.io/demo-01.html" src="about:blank;"></iframe>

[slide]
## Event对象下的方法-阻止默认事件 
- 非IE: `event.preventDefault()`  
- IE: `window.event.returnValue = false`   
````   

	<a class="btn" href="http://bj.58.com">58同城-北京</a>
	<a class="btn" href="http://sh.58.com">58同城-上海</a>
   
    var oBtn = document.getElementsByClassName("btn");
	 oBtn[1].addEventListener("click",function(event){
	      event.preventDefault();
	    },false)
```` 
<iframe data-src="https://wangminghuan.github.io/demo-02.html" src="about:blank;"></iframe>
[slide] 
## 事件委托

[slide] 
## 事件类型

[slide] 
## 模拟事件

[slide] 
## Vue中的事件

[slide] 
## React中的事件

[slide] 
## 问题

[slide] 
## 参考资料
- [DOM事件简介](http://blog.jobbole.com/52430/)
- [事件绑定、事件监听、事件委托](http://blog.xieliqun.com/2016/08/12/event-delegate/)
- [实例分析JavaScript中的事件委托和事件绑定](http://www.diguage.com/archives/71.html)