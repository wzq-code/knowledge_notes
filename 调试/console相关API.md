编码过程中，`console`的方法使用最多的无疑是`console.log`。下边就开始介绍`console`的其他方法和一些实用技巧。

----------------------------------------------------------------

#### `console.log`使用技巧

我们在使用`console.log`**Debug**的时候，如果输出多个变量的时候，我们可以使用对象属性简写的方式，让控制台输出的更清楚。

**例子**：

```javascript
const a = 1
const b = 2
const c = 3
console.log(a,b,c);
```

这段代码会直接输出a,b,c的值：也就是`1 2 3 `。如果我们需要使用很多`log`，这个时候就会分不清谁是谁了。

如果想要控制台显示的更清楚，可以一眼看出变量对应的值，我们可以改成如下代码：

```javascript
const a = 1
const b = 2
const c = 3
console.log("a:",a);
console.log("b:",b);
console.log("c:",c);
```

这样控制台就会输出：

```
a：1
b：2
c：3
```

这样就会很清晰的显示出变量所对应的值，`a`对应`1`，`b`对应`2`，`c`对应`3`。但是写起来还是比较麻烦的。这时我们可以使用对象属性值简写。将上述代码改写成如下为：

```javascript
const a = 1
const b = 2
const c = 3
console.log({a,b,c});
```

上述代码在控制台输出如下：

```javascript
{a:1,b:2,c:3}
```

#### `console.log`

##### 语法

```javascript
console.log(obj1);
console.log(obj1,/*...*/objN);

console.log(msg)
console.log(msg,subst1,/*...*/substN)
```

从上述代码中我们可以看到，`console.log`大概可以分为两种用法：

1. 单纯输出对象、字符串、数字等类型的变量
   1. 可以输出单个变量
   2. 可以输出多个变量，用逗号进行分隔
2. 输出一个字符串，第二个及以后的变量都是替代字符串，用来替代第一个变量字符串中的占位符。



**例子：**

```javascript
console.log(' %s + %s = %s', 1, 1, 2)
//输出 1 + 1 = 2
```

在上边的例子中`%s`就是**占位符**，第二个及以后的变量就是**替代字符串**。

1. 当占位符的数量**等于**替代字符串的数量时，替代字符串会按照顺序替代占位符。
2. 当占位符数量**小于**替代字符串的数量时，那么多出来的替代字符串就会被当做**普通变量**直接输出出来，其余替代字符串会正常替代。
3. 当占位符数量**大于**替代字符串的数量时，多出来的占位符会被直接输出出来，如占位符是`%s`，多出来的就是输出`%s`。

#### 占位符

占位符有以下五种：

1. 字符串：%s
2. 整数：%d
3. 浮点数：%f
4. 对象：%o或%O
5. CSS样式：%c

这里边前三种很好理解，就和`c`语言的用法是一样的。值得关注的是第四个和第五个占位符：CSS样式。这也就说明了我们可以控制log输出文字的样式。

**对象**

对象占位符的写法有两种，分别是`%o`和`%O`，在输出普通对象的时候，他们两个没有区别，如果输出DOM节点区别如下：

- 使用`%o`是打印节点的内容，包括子节点，我们日常不使用占位符也是输出这个
- 使用`%O`是打印节点对象属性

**CSS样式**

**例子：**

```javascript
console.log('%cWarning!', 'color:red;font-weight:bold;');
console.log('My Name is %cCUGGZ', 'color: skyblue; font-size: 30px;') 
```

CSS样式占位符只能影响在它**后边**的字符串样式，所以第二个`log`中，只有`CUGGZ`的样式被影响了。

可以通过一些字符画的转换网站进行转换，在控制台中打印字符画。[patorik](http://patorjk.com/software/taag)这个网站比较常用，可以在这个网站里转换字符画然后打印在控制台。

**例子：**

```javascript
Function.prototype.makeMulti = function () {
    let l = new String(this)
    l = l.substring(l.indexOf("/*") + 3, l.lastIndexOf("*/"))
	return l
}

let string = function () {
/* 
_               _                
| |             | |               
| |_   _ _ __   | | _____   _____ 
| | | | | '_ \  | |/ _ \ \ / / _ \
| | |_| | | | | | | (_) \ V /  __/
|_|\__, |_| |_| |_|\___/ \_/ \___|
__/ |                         
|___/                          
*/
}
console.log(string.makeMulti());


```



#### `console.warn`

`console.warn`在控制台中输出的内容前边有一个**黄色**三角形中间是一个感叹号的标志，代表着警告。用法与`console.log`一样，只是显示的样式不同。

#### `console.error`

`console.error`在控制台中输出的内容前边有一个**红色**三角形中间是一个感叹号的标志，代表着错误。用法与`console.log`基本相同，唯一不同的就是，`console.error`可以打印函数的调用栈。

```javascript
  function a(){
    console.error(1111);
  }
  function b(){
    a()
  }
  function c(){
    b()
  }
  c()
```

这个时候在控制台中会打印出函数调用栈的信息 ：**a->b->c**。

**小结**：这两个方法可以用在**封装一些库**的时候使用，当库的使用者在使用上出现错误，库开发者可根据错误在控制台中对使用者进行**警告和报错**，以便于使用者及时修正错误的使用方法。

#### `console.info`

与`console.log`基本相同，不同点是在**Firefox**上，会在输出信息的前边出现一个蓝色的三角形。



#### `console.time`&`console.timeEnd`&`console.timeLog`

**先上例子**：

```javascript
console.time('timer')
setTimeout(() => {
  console.timeLog('timer')
  setTimeout(() => {
    console.timeEnd('timer')
   },500)
},1000)
//timer: 1014.530029296875 ms  timeLog打印
//timer: 1518.182861328125 ms	timeEnd打印
```

**用途：**

三个方法搭配来计算代码执行时间。（不包含DOM渲染的时间，如需要计算DOM渲染时间，需要使用`window.performance.timing.domCompleted`等属性）

**参数：**

三个方法可以传递一个参数，参数为字符串，用来标记唯一的计时器。当代码中只有一个计时器的时候，也可以不传递参数，就是使用默认计时器。当出现多个计时器的时候，一定要传递参数！

**time**

这个方法是开启一个计时器，计时器开始计时。

**timeLog**

这个方法是在控制台中打印出**当前时间**距离计时器**开始时间**的**时间差**。

**timeEnd**

这个方法是关闭一个计时器，并结束计时，然后在控制台中打印出计算的时间。**计时器关闭以后，无法使用`timeLog`方法打印时间！**

#### `console.group`&`console.groupEnd`

**用途：**

将控制台信息进行分组，信息呈现的格式是一个树结构，这样复杂的输出就可以用分组进行隔离，开发者可以观察的更清晰。

```javascript
console.group();
console.log('First Group');
console.group();
console.log('Second Group')
console.groupEnd();
console.groupEnd();
```

`group`方法在控制台中呈现的树状结构信息是默认展开的。

`groupCollapsed`是默认折叠的，（从方法命中也能看出来。）

#### `console.count`

**用途：**

该方法的用途主要是在计算一些函数的调用次数，是否出现重复调用或者少调用的现象。

**参数：**

与`console.time`同理

**例子：**

```javascript
  function foo(){
    console.count('counter')
  }
  for(let i = 0 ; i <10 ; i ++){
    foo()
  }
  foo()
/*输出
counter: 1
counter: 2
counter: 3
counter: 4
counter: 5
counter: 6
counter: 7
counter: 8
counter: 9
counter: 10
counter: 11
*/
```

上述代码方法传递的参数是`'counter'`所以打印的时候也出现`'counter:'`，如果不传递参数就会打印`'default:'`。

####  `console.countReset`

这个方法的作用就是将计数器归零，可以配合`console.count`中的参数进行精准清除具体某个计时器。

```javascript
console.count(); 
console.count("a"); 
console.count("b"); 
console.count("a"); 
console.count("a"); 
console.count(); 
console.count(); 
  
console.countReset(); 
console.countReset("a"); 
console.countReset("b"); 
  
console.count(); 
console.count("a"); 
console.count("b");
```

#### `console.table`

**用途：**

适用于打印数组对象的属性。会将我们打印的数组打印成表格的形式。

**参数：**

第一个参数：就是我们要打印的数组（数组元素都是对象）。

第二个参数：就是打印内容的**表头**。

如果数组元素不是对象或者对象的属性和表头不对应，那么表格就会多出一列`value`，非相关的值在`value`列显示。

```javascript
const users = [ 
   { 
      "first_name":"Harcourt",
      "last_name":"Huckerbe",
      "gender":"Male",
      "city":"Linchen",
      "birth_country":"China"
   },
   { 
      "first_name":"Allyn",
      "last_name":"McEttigen",
      "gender":"Male",
      "city":"Ambelókipoi",
      "birth_country":"Greece"
   },
   { 
      "first_name":"Sandor",
      "last_name":"Degg",
      "gender":"Male",
      "city":"Mthatha",
      "birth_country":"South Africa"
   }
]

console.table(users, ['first_name', 'last_name', 'city']);

```



#### `console.clear`

清空控制台的内容。



#### `console.assert`

**用途：**

用于语句断言，当某种情况下要输出的时候，可以使用`console.assert`。

**参数：**

`expression`：条件语句，会被解析成`Boolean`类型，当值为`false`的时候，会输出`message`。

`message`：输出语句，可以是任意类型。

**例子：**

```javascript
console.assert(list.length<10,'list is too short')
```



#### `console.trace`

**用途：**

用于打印当前执行的代码堆栈调用情况。作用和上边所述的`console.error`显示堆栈情况是一样的，这个方法更加纯粹只用于该功能。

#### **`console.dir`**

`console.dir`方法可以在控制台中显示指定JavaScript对象的属性，并通过类似文件树样式的交互列表显示。它的语法如下：

```javascript
console.dir(object);
```

它的参数是一个对象，最终会打印出该对象所有的属性和属性值。 

在多数情况下，使用`console.dir`和使用`console.log`的效果是一样的。但是当打印元素结构时，就会有很大的差异了，console.log()打印的是元素的DOM结构，而`console.dir`打印的是元素的属性。



#### `console.memory`

这是一个属性而不是方法，我们可以打印该属性来观察内存的使用情况。

![image-20230812180744112](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20230812180744112.png)
