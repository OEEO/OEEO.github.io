---
layout: post
title:  jQuery 常用API分类
date:   2018-02-23
categories: JavaScript
tags: jq
---

## jQuery 要注意的点

* JQ对象与DOM对象区分。JQ对象用JQ的方法，DOM对象用DOM方法。不能互用。

* window.onload()是在建完DOM树之后，也是在所有图像和其他外部资源完整加载并在浏览窗口显示完毕之后，才执行。所以当某个资源要加载很久，会一直等待。只能在页面绑定一个。

  而jQuery的$(document).ready(function())，是在建完DOM树之后，就执行了。可以在页面绑定多个。

* jq对象与DOM对象互转。jq转DOM，用下标就可以。DOM转jq，用`$()`括起来就可以。

* 多个库共用有`$`有冲突时，用以下方法解决冲突：

  ```javascript
  var $j = jQuery.noConflict();//自定义一个快捷方式。jQuery依旧可用，$不再是jQuery的快捷方式
  ```

* `$('')`获取的永远是对象，即使dom中没有这个元素，判断是否存在用`.length > 0`来判断

* jq中的方法很多都可以读取值和设置值。通常括号里不填参数，则读取第一个匹配元素的值；括号里填参数则设置所有匹配元素的值。

  ​


## 常用API

### 选择器（只摘录常用的）

```javascript
//基本选择器
$('#id')//id  
$('element')//ele 
$('.class')//class
$('*') //所有元素
$('selector,selector')//多个选择器，逗号分隔；

//关系选择器
$('sel sel')//后代选择器
$('parent>child')//子代选择器
$('pre+next')//相邻兄弟选择器
$('pre~siblings')//后面兄弟选择器

//关系选择器替代方法，括号内可以再写选择器
.children()//子元素
.parent()//父元素
.parents()//祖先元素
.next()//下一个相邻兄弟
.nextAll()//后面所有相邻兄弟
.prev()//前一个相邻兄弟
.prevAll()//前面的所有兄弟
.siblings()//所有兄弟

//过滤选择器

//基本过滤选择器
:first//匹配找到的第一个元素
:last//匹配找到的最后一个元素
:not(sel)//匹配找到的除去not中的元素剩下的元素
:even//匹配找到的索引为偶数的元素、从0开始
:odd//匹配索引为奇数的元素，索引从0开始
:eq(index)//匹配指定索引值的元素
:gt(index)//匹配大于指定索引值的元素
:lt(index)//匹配小于指定索引值的元素
:animated// $(':animated')//匹配正在执行动画效果的元素
:focus//选取当前猎取焦点的元素。

//例如：只对不在执行动画效果的元素执行一个动画特效:
$("#run").click(function(){
  $("div:not(:animated)").animate({ left: "+=20" }, 1000);
}); 

//子元素过滤选择器
:nth-child(index/even/odd/eq)//与css的用法相似，index从 1 算起
:first-child  :last-child  :only-child

//内容过滤选择器
:contains('我们')//过滤选择器
:eq(i)//i从0算起，可以用于过滤jq匹配的元素集合
:empty//选择不包含子元素或文本的元素
:has(sel)//选择包含sel元素的元素
:parent//选择有子元素或文本的元素

:hidden//选择所有不可见的元素
:visible//选择所有可见的元素

//属性过滤选择器
[attribute]//选取拥有此属性的元素
[attribute="value"]//选取属性值为value的元素
	!="value"//值不等value
	^="value"//值以value开始
	$="value"//值以value结束
	*="value"//值含有value
	|="value"//值等于value或以它为前缀（该字符串后跟一个连字符“-”）的元素
	~="value"//选择属性用空格的值中包含一个给定值的元素
[attri][attri][attri]//满足多个条件

//表单对象属性过滤选择器
:enabled//选择所有可用的元素
:disabled//选择所有不可用的元素
:checked//选取所有被选中的元素（单选，多选）
:selected//选择所有被选中的选项元素（下拉列表）

//表单选择器
:input  //匹配所有input, textarea, select button元素
:text  //匹配所有单行文本框
:radio  //所有单选
:checkbox  //所有复选
:submit  //所有提交按钮
:button  //所有按钮
:reset  //所有重置按钮
:image  //所有图像按钮
:password  //所有密码框
```



### 常用方法（属性、样式）

```javascript
//创建元素
$('</div>').css('border','solid 1px #FF0000').html('createElement').appendTo($('#testDiv'));
//创建元素另一种方法
$('<div style="border:solid 1px #FF0000">动态创建的div</div>').appendTo(testDiv);

//添加元素
.prepend()//作为后代第一个元素
.prependTo()//作为后代第一个元素
.append()//作为后代最后一个元素
.appendTo()//作为后代最后一个元素
.after()//作为兄弟
.before()//作为兄弟
.insertAfter()//作为兄弟跟after功能一样，位置对调
.insertBefore()

//删除
.empty()//与.html('')效果一样
.remove()//删除元素，比empty好用的地方就是可以传递一个选择器表达式用来过滤将被移除的元素集合
.removeAttr()
.removeClass()
.detach()//删除节点，但保留这个元素的内存模型对象，即数据与事件都保留。

//替换
.replaceWith()//用括号内的内容替换匹配元素
.replaceAll()//与上功能一样，位置对调。

.wrap()//匹配的所有元素，分别单独用一个元素包裹
.wrapAll()//匹配的所有元素，用一个元素包裹，如果元素不连着，会拉到一起
.wrapInner()//匹配的所有元素的子元素，分别单独用一个元素包裹
.unwrap()//删除所有匹配元素的父元素

$('p').click(function(e) {
  alert(e.target.innerHTML)
})
p = $('p').detach();
$('body').append(p);

//查找匹配
ele.has('li')//查找有li后的的ele元素
.hasClass()//查找有某类的元素

is(':visible')//判断元素是否可见

//添加元素和文本
.html()
.text()
.val()

$('select').val(['选项1']);
$(':radio').val(['radio1']);
$(':checkbox').val(['check1','check2']);

.add()//往jq集合中添加元素

//选择关系元素
.eq()//下标第几个
.children()//子代元素
.find()//查找匹配后代元素
.parent()//所有匹配元素的父元素
.parents()//祖先元素，括号内可加选择器
.next()//紧邻的后面的同辈元素
.nextAll()//后面的所有同辈元素
.prev()//紧邻的前面的同辈元素
.prevAll()//前面的所有兄弟元素
.siblings()//同辈元素集合
$.contains(container,contained)//检查一个DOM元素是另一个DOM元素的后代

$('ul li').index('.sel-li')//指定元素在集合中的索引

//遍历jQuery对象
.each( callback )

//读取、设置、删除元素属性（已设置的属性）
.attr()//读取属性时，是读取jQuery对象的第一个。设置属性则是批量设置
.removeAttr()//对应的DOM属性是不会被删除的，只会影响DOM属性的值

//class相关
.addClass()//可以一次性添加多个类，空格分隔
.hasClass()//true or false
.removeClass()//不传入参数，则删除全部类
.toggleClass(class)//存在就删除一个类，不存在就添加一个类
.toggleClass(class,switch)//加入switch判断条件，为true时添加，false删除


//css样式
.css('color')//访问属性值
.css({color:'red',background:'blue'})//设置多个或单个样式
.css(color,red)//设置单个值

//区分attr、css，attr设置读取元素自带的属性，css设置读取style里的属性
alert("attr(\"width\"):" + $("#testDiv").attr("width")); //undifined
alert("css(\"width\"):" + $("#testDiv").css("width")); //auto(ie6) 或 1264px(ff)
alert("width():" + $("#testDiv").width()); //正确的数值1264
alert("style.width:" +  $("#testDiv")[0].style.width ); //空值

//宽高相关
.width(20) .height()//读取和设置元素内容的宽高、也可加单位px、em，默认px
.innerHeight()  innerWidth()  //元素内容宽高加上padding、不包括边框
.outerHeight(boolean)  .outerWidth(boolean)//包括边框、括号内为true时，还包括margin，默认为false

//位置相关
.offset().left  .offset().top//元素在当前窗口的偏移
.position().left  .position().top//元素相对父元素的偏移
.scrollTop(val)  .scrollLeft(val)//元素相对滚动条顶部、左边的偏移、或设置值

//data
$.data(ele, key, value)，用于设置读取html的data属性数据
```



### 事件与事件对象

```javascript
//与addEventListener
.one();//与bind类似，不过绑定的事件执行一次就立即解除绑定。
.bind(type,[data], fn);//事件绑定，[data]可以省略。
.unbind(type, fn);//解除事件绑定

//事件名称
.load()//加载
.unload()//卸载
.error()//错误
.resize()//改变尺寸
.scroll()//滚动条滚动


.focus()//元素获得焦点触发事件，不支持事件冒泡
.blur()//元素获得焦点触发事件，不支持事件冒泡
.focusin()//元素获得焦点触发事件，支持事件冒泡，
.focusout()//元素失去焦点触发事件，支持事件冒泡，
.change()//
//input元素监听到value值变化，失去焦点后触发change事件；单选，多选，用户作出选择时，立即触发；
//select元素，当用户作出选择时立即触发change
//textarea元素，当有改变时，失去焦点后触发change

.click()//点击事件
.dblclick()//双击事件


.keydown()//用户按下键盘下某按键时触发。一直按某按键会重复触发
.keypress()//用户按下一个按键，且产生一个字符时触发。（即按ctrl之类不会触发），一直按某按键会重复触发
.keyup()//用户释放某一个按键时触发

.mousedown()
.mouseup()
.mouseenter()//事件只会在绑定它的元素上被调用，而不会在后代节点上触发
.mouseleave()//事件只会在绑定它的元素上被调用，而不会在后代节点上触发
.mouseover()//会在后代节点触发
.mouseout()//会在后代节点触发
.mousemove()



.select()//只有input textarea可以触发，输入框中的文本被选中，并放开鼠标时触发
.submit()//


.hover(enter,leave)//enter,leave为两个函数，其实就是mouseenter与mouseleave的结合

//模拟操作触发事件或触发自定义事件、及给事件回调函数传递数据
.trigger(type,[data])

.triggerHandler()//只触发事件函数，不执行默认行为。
//例如input，focus事件绑定了一个函数，执行$('input').triggerHandler('focus')，input不会获得焦点，只是执行事件函数

//js事件对象的属性，需要使用jq方法时，用$()括起来转成jq对象即可。
e.type//事件类型
e.preventDefault()//阻止事件默认行为
e.stopPropagation()//阻止事件冒泡
e.target//获取触发事件的元素
e.currentTarget//获取冒泡到达的元素
e.pageX,e.pageY//鼠标相对页面的位置
e.which//按下了鼠标哪个键，左、中、右分别为1、2、3


//添加事件命名空间，便于管理
$('div').bind('click.plugin', function(){
  $(this).next().css({'background-color':'red'});
});
$('div').bind('mouseover.plugin', function(){
  $(this).next().show();
});
$('div').unbind('.plugin');//就会把所有添加了.plugin命名空间的事件删除

//两个click事件，一个click事件有命名空间，而另一个没有。而你想触发没有命名空间的事件时
.trigger('click!')//表示没有添加命名空间的click事件
```



### 动画

```javascript
//禁用所有动画
jQuery.fx.off = true;//禁用
jQuery.fx.off = false;//启用


//基本动画，集合了滑动和透明度渐变函数
.show(speed,[callback]);//显示，speed速度，callback是在动画执行完后执行
.hide(speed,[callback]);//隐藏
.toggle(speed,[callback]);//显示和隐藏切换，jq1.9后的版本已取消

//滑动动画
.slideDown(speed,[callback])//向下增大
.slideUp(speed,[callback])//向上减小
.slideToggle(speed,[callback])//来回切换，jq1.9后的版本已取消

//淡入淡出动画
.fadeIn(speed,[callback])//淡入
.fadeOut(speed,[callback])//淡出
.fadeTo(speed,opacity,[callback])//把所匹配的元素的不透明度渐变到指定不透明度

.animate({css},speed,callback)//自定义动画

.stop(clearQueue,gotoEnd)//两个值都为布尔值，1表示是否清空未执行完的动画队列。2表示是否直接将正在执行的动画跳转到末状态
//停止动画
//假设有三个动画按顺序执行
.stop()//停止第一个动画，第二个动画立刻执行
.stop(true)//如果同一元素调用多个动画方法，尚未被执行的动画被放置在元素的效果队列中。这些动画不会开始，直到第一个完成。当调用.stop()的时候，队列中的下一个动画立即开始。如果clearQueue参数提供true值,那么在队列中的动画其余被删除并永远不会运行
.stop(true,true)//当前动画停止，但该元素上的CSS属性会立刻修改成动画的目标值。

//反复触发事件是动画会累积，因此在先判断，元素是否处于动画状态 
$(ele).is(':animated')

//动画延迟
.delay(1000).animate()//延迟1秒再执行

$.each(array,callback(index,value))//遍历对象，数组
$.each(object,callback(property,value))
```



### Ajax

jQuery提供了几个用于发送Ajax请求的函数. 其中最核心也是最复杂的是`[jQuery.ajax( options )]`,所有的其他Ajax函数都是它的一个简化调用. 当我们想要完全控制Ajax时可以使用此结果, 否则还是使用简化方法如get, post, load等更加方便.

* load(url, [data], [callback])

  load方法能够载入远程 HTML 文件代码并插入至 DOM 中。 

  默认使用 GET 方式, 如果传递了data参数则使用Post方式. 

```javascript
$('contaner').load(url,[data],callback)//加载html到指定目标
$.get(url,data,callback,type)//请求地址，请求数据，回调函数，返回数据类型
$.post(url,data,callback,type)//请求地址，请求数据，回调函数，返回数据类型
$.getScript('test.js',callback)//需要时才加载js文件，加载完成执行回调函数
$.getJSON('test.json',callback(data))//需要时加载json文件，加载完成后执行回调函数
$.each(data,function(index,val){});//jq全局遍历函数，data是一个数组或对象，和forEach类似，i和v位置对调

//上述方法都可以通过$.ajax({option})实现
$.ajax()的参数option对象，常用属性如下

url//请求地址
type//string，GET,POST
timeout//NUmber，设置请求超时时间，此设置将覆盖$.ajax.Setup()
data//string or Object，发送到服务器的数据
dataType//返回的数据类型，json,jsonp,script,html,xml,text,jQuery
beforeSend//发送请求前调用函数
complete//请求完成调用函数
success//请求成功调用函数
error//请求失败调用函数
global//默认为true，表示是否触发全局ajax事件。AjaxStart()，AjaxStop()可用于控制各种Ajax事件，全局函数

例如：
//<div id="loading">加载中...</div>
$('#loading').ajaxStart(function(){
  $(this).show();
});
$('#loading').ajaxStop(function(){
  $(this).hide();
});

//表单序列化元素
.serialize()//序列化为字符串
.serializeArray()//序列化为json格式的数据，是一个jq对象，可用$.each()遍历，用于购物清单确认？
data:$('form').serialize()//把目标form的填写内容序列化为字符串。

$.param()//它是serialize()方法的核心，用来对一个数组或对象按照key/value进行序列化
```

提示:我们要时刻注意浏览器缓存,  当使用GET方式时要添加时间戳参数` (net Date()).getTime()` 来保证每次发送的URL不同, 可以避免浏览器缓存。

提示: 当在url参数后面添加了一个空格, 比如"  "的时候, 会出现"无法识别符号"的错误, 请求还是能正常发送. 但是无法加载HTML到DOM. 删除后问题解决。
