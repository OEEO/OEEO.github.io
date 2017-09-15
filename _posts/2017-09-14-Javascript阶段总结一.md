---
layout: post
title:  Javascript 阶段总结一
date:   2017-09-14
categories: Summary
tags: Javascript
---
* content
{:toc}

## DOM&BOM

### 基本概念

刚开始接触前端的时候，对这两个词总是一知半解，迷迷糊糊的。其实很好理解。

DOM（Document Object Model）：文档对象模型，也就是把『文档』当做一个『对象』来看待。

DOM有各种各样的属性，以及方法。要访问文档中的各个组件，可以通过document.attribute，及document里的方法来实现。

而BOM除了可以访问文档中的组件之外，还可以访问浏览器的组件，比如导航条，历史记录等等。



### 操作DOM常用到的一些方法、属性

#### 操作子节点

`.childNodes` 常用于遍历节点（会包含换行，空白，注释），一般要配合nodeType来使用。

nodeType = 1、2、3；分别表示元素、属性、文本节点。

相关的属性还有parentNode，nextSibling，previousSibling，childNodes，firstChild，lastChild

`.children` 则只匹配元素节点。（但是在ie678还会包含注释节点），还有firstElementChild，lastElementChild。

.nodeType	.nodeName	.nodeValue，可以分别获得节点的类型，名字，值

#### 创建元素

一般而言有两种方法创建元素。

* innerHTML
  * 用innerHTML会先把元素清空，然后再插入新的节点。所以有时需要再创建一个容器来包含要新建的节点。

  * 每次添加元素，都会造成浏览器回流，性能损失。因此，要选用一个临时变量来保存创建的节点，最后一次性添加。

    注：关于重绘（repaint）与回流（reflow）

    repaint，就是浏览器得知元素产生了不影响排版的情况下后对这个元素进行重新绘制的过程。例如我们改变了元素的颜色，加个下划线等。

    reflow, 浏览器得知元素产生了对文档树排版有影响的样式变化，对所有受影响的dom节点进行重新排版工作
* document.createElement()
  * 这个方法与innerHTML相似，不过不会清除原有的元素。可以结合appendChild()及insertBefore()使用。
  * 每次添加元素，都会造成浏览器回流，性能损失。不过也有一个专门的方法应对这种情况。就是创建一个**document.createDocumentFragment()**临时节点，将新建的加点加进去，最后再把这个临时节点加入到目标。这个方法创建的临时节点，不是文档树的一部分，它保存在内存中，所以不会造成回流。


#### 删除元素/节点

removeChild();

replaceChild(newnode, oldnode);



## 三种提示框

第一种：alert()

第二种：confirm()

```javascript
var a = confirm('你是一个好人？');
if(a === true){   }  else{   };
```

第三种：prompt()

```javascript
var name = prompt('请输入你的名字？');
if(name !== null){  } else{   };
//在prompt提示框中，当点击取消时，返回值就是null，点击确定则返回你输入的内容
```



## 三种宽度

`.clientWidth`：内容可视区宽度

`.scrollWidth`：内容的实际宽度

`.offsetWidth`：元素的实际宽度



## true & false

下面列出的值被当作假：false、null、undefined、空字符串''、0、数字NaN

其他所有值都被被做真。

在做相等比较时，最好只用===和!==



## undefined

* 变量被声明了，但没有赋值，就等于undefined。
* 调用函数时，应该提供的参数没有提供，该参数等于undefined。


* 对象没有赋值的属性，该属性的值为undefined。
* 函数没有返回值时，默认返回undefined。




## 判断数据类型的常用方法

- typeof 操作符，用于判断number，string，boolean，undefined，function

- instanceof 运算符  a instanceof b，a是否是b的一个实例，会判断原型链。多框架不适用

- isPrototypeof(instrance)，区别于instanceof，就是当用Object.create()来创建新实例，又没有重新制定constructors时，instanceof 会出错，而这时照样可以用isPrototypeof()

  ```javascript
  var A = {
   //something
  }
  var B = Object.create(A);
  var C = Object.create(B);

  console.log(A.isPrototypeOf(C));  // true
  console.log(C instanceof A);      //TypeError
  ```

- hasOwnProperty()方法，是否是自己的实例，不判断原型链

- Array.isArray(arr)；用于判断arr是否为数组；（ie9+）

- Object.prototype.toString.call(obj) ；返回[Object class]，class是obj的数据类型，这个可用于判断对象的数据类型，例如arr是一个数组。跨框架可用。

  ```javascript
  Object.prototype.toString.call(arr);// [object Array]，记住首字母大写
  ```




## 对象

### 对象&数组的比较都是引用的比较

因此，在比较对象&数组的值是否全等时，不能直接比较，要先用toString()，把它们转换成字符串再比较。

```javascript
var arr1 = [1,2],arr2 = [1,2];
console.log(arr1 === arr2);//false
console.log(arr1.toString() === arr2.toString());//true

var a = {}, b = {}, c = {};//a、b、c每个都引用一个不同的空对象
a = b = c = {};或c = {}; b = c; a = b; //a、b、c都引用同一个空对象
```

### ECMAScript中有两种属性：数据属性和访问器属性。

#### 数据属性
数据属性有四个描述其行为的特性：
- **[[configurable]]**：表示是否能够通过delete操作符删除属性，以及能否修改属性的这四个特性。当直接在对象上定义属性时，这个值默认为true
- **[[Enumerable]]**：表示能否通过for-in循环返回属性。当直接在对象上定义属性时，这个值默认为true
- **[[Writable]]**：表示能否修改属性的数据值Value。当直接在对象上定义属性时，这个值默认为true
- **[[Value]]**：包含这个属性的数据值。这个特性默认值为undefined。

要修改属性的默认特性的值，必须使用Object.defineProperty()或Object.defineProperties()方法(修改一个或多个特性)。这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。 而由Object.defineProperty()或Object.defineProperties()创建的特性，默认值都为false。

```javascript
var person = {
    name:'wang',
    age:18
};
Object.defineProperty(person,'name',{
    writable:false;
});
person.name = 'li';
alert(person.name);//wang
```

#### 访问器属性
访问器属性不包含数据值，它们包含一对getter和setter函数（不过，这两个函数也不是必须的）。
访问器属性有四个特性：
- **[[configurable]]**：表示是否能够通过delete删除属性，能否修改属性的这四个特性。当直接在对象上定义属性时，这个值默认为true
- **[[Enumerable]]**：表示能否通过for-in循环返回属性。当直接在对象上定义属性时，这个值默认为true
- **[[Get]]**：在读取属性时调用函数。默认值为undefined。
- **[[Set]]**：在写入属性时调用的函数。默认值为undefined。

注：访问器中没有Writable和Value这两个特性，因此不要设置这两个的值，否则出错。


```javascript
var book = {
    _year : 2004,
    edition : 1
};
Object.defineProperty(book,"year",{ 
    get : function () {
        alert(this._year);
    },
    set : function (newValue) {
        if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        }
    }
});
book.year;      // 弹出窗口，显示 2004
book.year = 2005;
console.log(book.edition);   // 2
```

专门读取数据属性和访问器属性的特性的值的方法：
Object.getOwnPropertyDescriptor()，这个方法接收两个参数，属性所在的对象，属性名称。

```javascript
var girl = {name: "zyj"};
console.log(Object.getOwnPropertyDescriptor(girl,"name"));
// Object {value: "zyj", writable: true, enumerable: true, configurable: true}
Object.defineProperties(girl,{
  name:{
    writable: false
  },
  age:{
    writable: true,
    value: 22
  }
});
console.log(Object.getOwnPropertyDescriptor(girl,"name"));
// Object {value: "zyj", writable: false, enumerable: true, configurable: true}
console.log(Object.getOwnPropertyDescriptor(girl,"age"));
// Object {value: 22, writable: true, enumerable: false, configurable: false}
var descriptor = Object.getOwnPropertyDescriptor(girl,"age");
console.log(descriptor.value);         // 22
console.log(descriptor.configurable);  // false
console.log(descriptor.writable);      // true
console.log(descriptor.get);           // undefined
console.log(descriptor.set);           // undefined
```



## 数组

数组的length的值，是这个数组最大整数属性名加1。

```javascript
var arr = [];
arr[100] = 'a';
console.log(arr.length);//101
```

### 判断是否为数组的方法

- instanceof ，在只有一个全局执行环境时有效，如果网页包含多个框架，那实际上存在多个全局执行环境时，无效。

- Array.isArray()，克服了instanceof的问题。不过要在ie9+以上才能用。因此存在浏览器兼容问题

- Object.prototype.toString.call(arr) === '[object Array]';最好的方法。可以写下面函数，或者直接给Array对象添加一个方法 isArray

  ```javascript
      (function(){
          if(!Array.isArray){
              Array.isArray = function(arr){
              return Object.prototype.toString.call(arr) === '[object Array]';
              }
          }
      })();
  ```

  ​

### 判断Number、转换成Number

- 常用的两个将字符串转数字的方法Number()与parseInt()区别

  - Number()，这个方法，是将一个字符串数或true，false，null转换成数字，其它转成NaN
  - parseInt()，把一个字符串转换成整数的函数。遇到非数字时，停止解析。有数返回数，没数返回NaN，解析数字时，还会默认根据数字前面的符号转换进制。例如'08'就会根据前面的0转换成八进制，又因八进制中没有数字 8，就会解析成0。因此在用这个方法的时候，最好添加上进制。例如：parseInt('08', 10);就会根据10进制来

- 判断一个值是否可用作数字的最佳方法是使用isFinite()函数。它会筛选掉NaN和Infinity，不过isFinite()也会像Number()一样试图把一个运算数转换为一个数字。

  ​

### 数组的操作方法整理

- 改变原数组的操作方法：

  - 尾部      加：push(arr[i])，      减：pop()；
  - 头部      加：unshift(arr[i])，   减：shift()；
  - 批量删除，插入。splice(index, howmany,item1,......itemX)
  - reverse()反转数组
  - sort()排列数组

- 不改变原数组的操作方法，返回新数组：

  - join('')    将数组串成字符串
  - slice()    截取部分数组，返回新数组          
  - concat()    合并数组
  - map()   遍历数组，做操作，形成数据，然后返回一个新数组，如果没设返回值，则返回一个等长的数组，数组每一项的值都是undefined

  ```javascript
  var arr = [1,[2,3],[[4,5],6],7];//把数组变成一维数组
  console.log(arr.toString().split(',').map(function(x){
  	return Number(x);
  }));//返回[1,2,3,4,5,6,7]
  console.log(arr.toString().split(',').map(function(x){
  	alert(x);
  }));//分别弹出1，2，3，4，5，6，7，最后返回[undefined,undefined.....]7项
  //因为map()方法返回的是一个数组，方便做链式操作。
  ```

  - forEach()  遍历数组，做操作，然后返回undefined   
  - filter()   判断数组的每一项是否符合某个条件，为true时，返回该项，最后形成新数组
  - every()   判断数组的每一项是否附合某一条件，全部附合，返回true，否则false
  - some()    判断数组是否有一项附合某一条件，有一项附合，返回true，否则false

  **注：**由于map()方法写起来方便，所以能用map的地方尽量用map，但是当遍历需要中断时，还是需要用for()。 

### 关于forEach()，filter()，map()

它们的回调函数，都有三个参数，而不只是平时常用的一个参数。

例如，你想数组去重，可以这样用。

```javascript
var arr = [1,1,2,1,2,1,3,4,5,6];
arr = arr.reverse().filter(function(v,index,arr){
  return arr.indexOf(v,index + 1) === -1;
}).reverse();//[1,2,3,4,5,6]
```



## 字符串

### 一些技巧

num + '';  可以将数字转换成字符串

num - 0;  可以将字符串形式的数字转换成数字

toString(num);  可以通过填num转换数值的进制表示形式，转换的结果也是字符串，例如：

```javascript
var a = 2;
console.log(a.toStirng(2));//10 
```

### 字符串的操作方法整理

- 返回字符串中某字符的位置或字符编辑的方法

  - str.charAt(index)；返回str中下标为index的字符，如果index超出范围，则返回''

  - str.charCodeAt(index)；返回str对应下标的字符的unicode编码数字。

    //注：fromCharCode(num)；则返回对应unicode编码数字num的字符。

  - string.indexOf(str)；返回字符str在string中首次出现的位置，如果没匹配，返回-1

  - lastIndexOf()：和indexOf()相似，不过是从末尾开始找

- 搜索匹配，替换字串，可以结合正则使用

  - match()，在字符串内检索指定值。如果有匹配，则返回检索到的结果（数组），数组长度匹配检索到的数量；如果没有匹配到，则返回null。
  - replace()，检索，并替换检索到的结果。返回替换后的字符串
  - search()，返回检索到的结果的下标，没有检索到则返回-1

- 截取字符串片段

  - slice(start,end)；包括start，不包括end，如果只传入一个参数，则从参数开始到尾
  - substring(start,end)；和slice相似，就是end用使用负数时会自动转为0
  - substr(start, howmany)；与前两者不同，这个方法第2个参数是要截取的长度

- 大小写转换

  - toUpperCase()
  - toLowerCase()



## JSON数据常用处理方法

- **JSON.parse()**方法用于从一个JSON格式字符串解析出javascript类型的数据

  > 语法：JSON.parse(text[, reviver])
  >
  > - text:必需， 一个有效的 JSON 字符串。
  > - reviver: 可选，一个转换结果的函数， 将为对象的每个成员调用此函数。
  > - JSON属性名称必须用双引号包裹；最后一个属性后面不能有逗号。
  > - 若被解析的 JSON 字符串是非法的，则会抛出 一个语法错误 异常。
  > - JSON.parse() 不允许用逗号作为结尾

```javascript
JSON.parse('{}');              // {}
JSON.parse('true');            // true
JSON.parse('"foo"');           // "foo"
JSON.parse('[1, 5, "false"]'); // [1, 5, "false"]
JSON.parse('null');            // null
JSON.parse('{name:tom}');      //Unexpected token 
JSON.parse('{name:"tom"}');    //Unexpected token 
JSON.parse('{"name":tom}');    //Unexpected token 
JSON.parse('{"age":undefined}');//Unexpected token 
JSON.parse('{"name":"tom"}');  //{name: "tom"}
JSON.parse('{"age":10}');      //{age: 10}
JSON.parse('{"age":null}');     //{age: null}
var str='{"p": 5}';
JSON.parse(str, function (k, v) {
    if(k === '') return v;     // 如果到了最顶层，则直接返回属性值，
    return v * 2;              // 否则将属性值变为原来的 2 倍。
});                            // { p: 10 }
```

- **JSON.stringify()**方法用于将一个javascript类型的数据转换成JSON格式字符串

  > 语法：JSON.stringify(value[, replacer [, space]])
  >
  > - value:一个有效的javascript值。
  >   replace:(可选)
  >
  > > - 如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；
  > > - 如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；
  > > - 如果该参数为null或者未提供，则对象所有的属性都会被序列化；
  >
  > - space:(可选)
  >
  > > - 如果参数是个数字，它代表有多少的空格；上限为10。改值若小于1，则意味着没有空格；
  > > - 如果该参数没有提供（或者为null）将没有空格;



## 正则表达式

* 正则表达式中添加变量的方法，用构造函数的方式创建正则表达式

  例如创建一个变量i重复三次的正则表达式

  ```javascript
  var str = new RegExp( i + '{3}' );
  ```

* 用前瞻判断正则表达式内容里是否含有什么

  例如一道题：匹配输入的内容password必须是，只能包含数字和字母，长度至少8位，而且必须至少有1个数字，1个小写字母，1个大写字母。

  ```javascript
  /^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z]{8,}$/.test(password);

  //三个正向前瞻条件并列，确保至少有1个数字，1个小写字母，1个大写字母
  ```

  ​

## 算法

### 排序算法

* 冒泡排序

  ```javascript
      function bubbleSort(arr){
      	for (var i = 1; i < arr.length; i++) {
      		for (var j = 0; j < arr.length - i; j++) {
      			var temp = 0;
      			if (arr[j] < arr[j + 1]) {
      				temp = arr[j];
      				arr[j] = arr[j + 1];
      				arr[j + 1] = temp;
      			}
      		}
      	}
      	return arr;
      }
  ```

  ​

* 快速排序

  ```javascript
  function quickSort(arr, left, right) {
      var len = arr.length,
          partitionIndex,
          left = typeof left != 'number' ? 0 : left,
          right = typeof right != 'number' ? len - 1 : right;

      if (left < right) {
          partitionIndex = partition(arr, left, right);
          quickSort(arr, left, partitionIndex-1);
          quickSort(arr, partitionIndex+1, right);
      }
      return arr;
  }

  function partition(arr, left ,right) {     //分区操作
      var pivot = left,                      //设定基准值（pivot）
          index = pivot + 1;
      for (var i = index; i <= right; i++) {
          if (arr[i] < arr[pivot]) {
              swap(arr, i, index);
              index++;
          }        
      }
      swap(arr, pivot, index - 1);
      return index-1;
  }

  function swap(arr, i, j) {
      var temp = arr[i];
      arr[i] = arr[j];
      arr[j] = temp;
  }
  ```

  ​



## 性能优化

* 一般来说，减少请求数量通常都是在性能优化时首先考虑的。
* 只要是查询DOM中的某些元素，浏览器都会搜索整个DOM树，从中查找可能匹配的元素。所以要避免重复使用搜索。可以采取先赋值给变量，后面就不用搜索，直接用变量就可以了。
* 减少浏览器回流



