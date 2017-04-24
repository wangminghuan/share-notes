[slide]
## DOM事件
## 王明欢
## 2017-04-19

[slide]

## 一、什么是事件

[slide]
javaScript 与 HTML 之间的交互是通过事件实现的。事件，就是文档或浏览器窗口中发生的一些特定的交互瞬间。可以使用侦听器（或处理程序）来预订事件，以便事件发生时执行相应的代码。

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

## 3.4 事件监听的取消  

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
## 3.5 事件监听与取消的兼容写法：  
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

## 四、Event对象

[slide]
- 事件第一次触发的时候被创建
- 作为第一个参数传递给事件监听的回调函数(非IE)
- IE下通过 window.event访问
[slide]
- click事件的event  
![DOM2-事件流](../img/03.png)

[slide]
## 4.1 Event对象下的方法-阻止冒泡  

- 非IE: `event.stopPropagation()/event.stopImmediatePropagation()`
- IE: `window.cancelBubble = true` 

```` 
	var btn = document.getElementById("btn");
	var wrap= document.getElementById("wrap");
	 btn.addEventListener("click",function(event){
	    //event.stopPropagation();
		//event.stopImmediatePropagation();
	       alert("我是监听函数1") },false) 
	 btn.addEventListener("click",function(event){
	       alert("我是监听函数2")},false) 
	 wrap.addEventListener("click",function(event){
	       alert("我是父级监听函数") },false) 
	   
```` 
<iframe style="height:105px" data-src="https://wangminghuan.github.io/demo-01.html" src="about:blank;"></iframe>

[slide]
## 4.2 Event对象下的方法-阻止默认事件 
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
<iframe style="height:100px" data-src="https://wangminghuan.github.io/demo-02.html" src="about:blank;"></iframe>
[slide] 
## 五、事件委托（event delegation）

[slide] 
- 利用冒泡原理，使用一个事件监听器去监听大量的DOM节点的事件
- 节约内存，提高性能，易于维护
- 举例：
```` 
	for(let i=0;i<oLi.length;i++){
	    oLi[i].addEventListener("click", function(){
	        alert(this.getAttribute("data-index"))
	    }, false)
	  }
	oUl2.addEventListener("click", function(e){
	   if(e.target.tagName=="LI"){
	     alert(e.target.getAttribute("data-index"))
	   }
	}, false)
```` 
<iframe style="height:200px" data-src="https://wangminghuan.github.io/demo-03.html" src="about:blank;"></iframe>

[slide] 
## 六、事件类型

[slide] 
## 6.1 点击事件（click）
- 单击鼠标左键or回车键触发
- 移动端click 300ms延迟（双击缩放）
- ([300 毫秒点击延迟的来龙去脉](https://thx.github.io/mobile/300ms-click-delay))

[slide] 
## 6.2 触摸事件（touch）

- 点击操作: touchstart -> touchend -> mousedown -> mouseup -> click
- 滑动操作: touchstart -> touchmove -> touchend
[slide] 
- React-解决300ms延迟 (react-fastclick)
```` 
	import initReactFastclick from 'react-fastclick';
	initReactFastclick();
	class ListItem extends Component{
	  constructor(props){
	     super(props);
	  }
	  render(){
	    return (
			 <li 
	          onClick={() => this.props.handleClick(this.props.val, this.props.name)}
	         ></li>
	  )}}
```` 

[slide] 
## 6.3 load事件
- DOM加载完毕-window.onload
- 图片加载完毕-img.onload
```` 
    //图片预加载功能
	var perLoadImage=function(arr,callback){
	   
	   var imageObjArr=[],hasloadImageLen=0;
	   var __arr=isArray(arr)? arr:[arr],
	       __callback=callback || function(){};
	   for (var i=0,len=__arr.length;i<len;i++){
	        imageObjArr[i]=new Image();
	        imageObjArr[i].src=__arr[i];
	        imageObjArr[i].onload=function(){
	           hasloadImageLen++;
	           hasloadImageLen == arr.length && __callback(__arr)
	        }
	        imageObjArr[i].onerror=function(){
	           hasloadImageLen++;
	           hasloadImageLen == arr.length && __callback(__arr)
	        }
	   }
	}
```` 
[slide] 
## 6.4 contextmenu 事件
- 单击鼠标右键触发，可以调出上下文菜单
- 通过阻止默认事件，实现自定义右键菜单
<iframe data-src="https://wangminghuan.github.io/demo-04.html" src="about:blank;"></iframe>

[slide] 
## 6.5 拖放事件（drag/drop）
- HTML5标准一部分，任何元素都能够拖放
- drop不触发时: dragstart -> drag -> dragenter -> dragover -> dragleave -> dragend
- drop触发的时候: dragstart -> drag -> dragenter -> dragover -> drop -> dragend

[slide] 
````
    var oDiv1=document.getElementById("div1");
    var oDiv2=document.getElementById("div2");
	oDiv1.addEventListener("drop", function(e){
	  this.appendChild(oDiv2)
	}, false)
	oDiv1.addEventListener("dragover", function(e){
	  e.preventDefault();//dragover时阻止默认事件，否则drop事件不会触发
	}, false)
	oDiv2.addEventListener("dragstart",function(){
	   this.style.backgroundColor="blue"
	}, false)
	oDiv2.addEventListener("dragend",function(){
	   this.style.backgroundColor="red"
	}, false)
````
<iframe data-src="https://wangminghuan.github.io/demo-05.html" src="about:blank;"></iframe>
[slide] 
## 6.6 其他事件
- 焦点事件（focus,blur）
- 键盘事件（keydown,keypress,keyup）
- 鼠标事件（dblclick,mousedown,mouseup,mouseover...）
- 设备事件（orientationchange, deviceorientation）
- UI事件（resize,scroll）
- 动画事件（transitionend，animationend）
[slide] 
## 七、模拟事件
[slide] 
## 7.1 非IE下的模拟事件步骤：
- 1.创建event对象：document.createEvent(事件类型) 
	- UIEvents：一般化的UI 事件。
	- MouseEvents：一般化的鼠标事件。
	- MutationEvents：一般化的DOM 变动事件
	- HTMLEvents：一般化的HTML 事件

- 2.初始化event对象：
	- 不同类型事件方法名不同
	- initMouseEvent()、initKeyEvent()等

- 3.触发事件：dispatchEvent()
[slide] 
````
	//创建event
	var event = document.createEvent("MouseEvents");
	//初始化event
	event.initMouseEvent("click",true,true,document.defaultView,0,0,0,0,0,false,false,false,false,0,null);
	//触发事件
	btn.dispatchEvent(event);
````
<iframe  data-src="https://wangminghuan.github.io/demo-06.html" src="about:blank;"></iframe>
[slide] 
## 7.2 IE下的模拟事件
````
	//创建事件对象
	var event = document.createEventObject();
	//初始化事件对象
	event.screenX = 100;
	event.screenY = 0;
	event.clientX = 0;
	event.clientY = 0;
	event.ctrlKey = false;
	event.altKey = false;
	event.shiftKey = false;
	event.button = 0;
	//触发事件
	btn.fireEvent("onclick", event);
````
[slide] 
## 八、React中的事件

[slide] 
- 事件处理程序通过"合成事件"层（synthetic event system）传递
- 以事件委托的方式绑定到组件最上层
- 组件卸载时自动销毁绑定的事件
- 命名上与原生的JavaScript规范一致
- React绑定事件处理器的语法和HTML语法非常相似
- stopPropagation()、preventDefault()与原生js规范一致，且跨浏览器兼容

[slide] 
```` 
class ListItem extends Component{
  constructor(props){
     super(props);
     this.handleClick = this.handleClick.bind(this);
  }
  handleClick(index,e){
    e.stopPropagation();
	//do something...   
  }
  render(){
    return (
      <ul>
        {this.props.arr.map((child,index)=>{
          <li 
           key={index}
           onClick={(index,e)=>this.handleClick(index,e)}
          >{child.text}</li>
        })}
      </ul>
      )}
}
```` 
[slide] 
## 九、Vue中的事件
- 组件卸载时自动销毁绑定的事件  
- 命名上与原生的JavaScript规范一致
- 通过v-on指令或事件语法糖`@`为DOM元素绑定事件
- 通过修饰符阻止冒泡/默认事件
[slide] 
```` 
	<template>
	  <ul class="dy-test">
	     <li v-for="(child, index) in arr" @click.stop="handleClick(index)">{{child.text}}</li>
	   </ul>
	</template>
	
	<script>
	export default{
	   props: {     
	      arr: Array
	  },
	  methods: {
	   handleClick (index) {
	     //do something...       
	    }
	  }
	}
	</script>
```` 
[slide] 
## 十、问题
- 执行事件程序的三种方式
- 待定
[slide] 
## 参考资料
- [DOM事件简介](http://blog.jobbole.com/52430/)
- [事件绑定、事件监听、事件委托](http://blog.xieliqun.com/2016/08/12/event-delegate/)
- [实例分析JavaScript中的事件委托和事件绑定](http://www.diguage.com/archives/71.html)
- [React源码解读系列 -- 事件机制](http://www.open-open.com/lib/view/open1488868982317.html)