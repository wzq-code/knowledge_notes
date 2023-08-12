#### `React`和`ReactDOM`是如何工作运行的？

---------------

当你使用React的时候，你可能会使用`JSX`进行构建应用程序。`JSX`是一个基于标签的`JavaScript`语法，它的语法规则看起来像`HTML`。`React`元素（`React element`）是在`JSX`和继续学习`React`之前需要掌握的最小和最基本的单位。换句话说，在学习`JSX`和继续深入学习`React`之前，我们要对`React`元素有基本的了解。

**注意：**为了了解`React`在浏览器中是如何运行的，我们需要两个库：`React`和`ReactDOM`。`React`库是负责创建视图的，而`ReactDOM`库是负责浏览器中渲染UI的。

> ```
> React: https://cdnjs.com/libraries/react
> ReactDOM: https://cdnjs.com/libraries/react-dom
> ```

引入这两个库在你的主要`JavaScript`文件之前。在了解`React`如何运行的同时，我们需要使用`React`和`ReactDOM`创建一个小型应用。简单起见，这个应用只包含***Index.html***和***main.js***两个文件。`React`库应该使用开发版本，这样在控制台是可以看到警告和错误的。

```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="https://www.unpkg.com/browse/react@18.2.0/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@18.2.0/umd/react-dom.production.min.js"></script>
  <script src="./main.js"></script>
</head>
<body>
  
</body>
</html>
```



### React element

先放概念：元素（`element`）是组成`React`应用的最小的单位。元素是一个普通的对象，它根据`DOM`节点描述了我们想要出现在页面上的东西。

HTML只是一组最终会变成`DOM`元素的命令。假如你需要写一个`JS`框架或者库的`HTML`的代码。比如使用`vue2`使用模板语法来构建DOM树。我们先使用HTML的语法来创建一个`HTML`层级结构来看下代码和效果：

```html
<section class="js-section">
  <h1>JavaScript Libraries and Frameworks</h1>
  <ul class="list-lib-frameworks">
    <li>React.js</li>
    <li>Angular</li>
    <li>Vue.js</li>
    <li>Node.js</li>
    <li>underscore.js</li>
  </ul>
</section>
```

上述`HTML`代码最后会渲染成一个`DOM`树，根节点是`section`，它有两个子节点，一个是`h1`，一个是`ul`，`h1`的子节点是文本节点。`ul`的子节点是5个`li`节点，`li`的子节点是文本节点。



在过去，网站是由多页面组成的，用户点击链接，浏览器请求一个新的`HTML`页面并且重新创建`DOM`。但是在`AJAX`发明之后，出现了单页面应用，也就是`SPA`（single page application）。在单页面应用中，浏览器会加载初始的`HTML`文档，然后用户通过点击链接进行导航的时候，浏览器会发送请求并且浏览器只会更新一部分DOM。当用户点击链接进行导航的时候，从观感上看，是从一个页面跳转到另一个页面了，但实际上是一直保持在一个页面。`JavaScript`摧毁旧的UI，然后创建新的UI，这就是`JavaScript`的幕后工作。那么`JavaScript`在更新UI方面是如何运行的呢？



#### JavaScript如何更新DOM?

`JavaScript`使用**DOM API**更新和操纵`DOM`节点。`DOM API`是`JavaScript`用来操作`DOM`的对象的集合。操作指的就是`CURD`,也就是增删改查，如果你想要构建一个`HTML`页面，也可以使用`JavaScript`进行构建：

```
const root = document.querySelector( 'body' );

function createListElement() {
	const libsAndFrameworksNames = ['React.js', 'Angular', 'Vue.js',
	'Node.js', 'Underscore.js'];
	
	const ul = document.createElement( 'ul' );
	ul.classList.add( "list-lib-frameworks" );

	libsAndFrameworksNames.forEach( function appoendToUnorderedList( name ) {
		const listElement = document.createElement( 'li' );
		listElement.innerText = name;
		ul.appendChild( listElement );
	} );

	return ul;
}

function createWebPageWithJavaScript( root ) {
	// PARENT ELEMENT
	const parent = document.createElement( 'section' );
	parent.classList.add( 'js-section' );
	
	// HEADING ELEMENT
	const heading = document.createElement( 'h1' );
	heading.innerText = 'JavaScript Libraries and Frameworks';

	// UNORDERED LIST ELEMENT
	const unorderedListElement = createListElement();

	// APPEND HEADING AND UNORDERED LIST ELEMENT TO PARENT
	parent.appendChild(heading)
	parent.appendChild( unorderedListElement );

	// APPEND PARENT TO ROOT ELEMENT
	root.appendChild( parent );
}

createWebPageWithJavaScript( root );
```



`React`是一个被设计用来和`DOM`交互的框架，从现在来看我们不直接更新`DOM`，而是告诉`React`我们要更新`DOM`.`React`将通过我们给出的命令为我们负责元素的渲染。

> 在DOM和React之间有许多相似点，比如他们都由节点组成。DOM是由DOM节点组成，React是由React element 组成的。看起来很相似，实际上很不同。



##### 创建React element

就像前边所说的，`React` `element`是`React`中最小的单位。`React`元素是在内存中描述`DOM`元素的`JavaScript`对象。我们可以通过使用`React`的方法**`createElement`**来创建一个`React`元素。

**语法：**

```javascript
React.createElement(type,[props],[...children])
```

**参数：**

`type`：`type`就是你想要创建元素的类型，如`div`，`p`，`li`等等。`type`也可以是其他的`React`元素。

`props`：它是一个`JavaScript`对象，包含构建DOM需要的属性和数据。

`children`or`content`：`React`元素可能会有`children`或者`content`来显示其他的嵌套的元素，是节点的内容。

**例子：**如何创建`li`元素

```
const element = React.createElement("li",null,'React.js')
console.log(element)
```

<img src="C:\Users\86150\Desktop\读书笔记\public\hrarw\reactElementLi.jpg" alt="reactElementLi" style="zoom:66%;" />

在上述例子中，我们给方法里传递了三个参数，如下所示：

**li：**它代表着我们创建`React`元素的类型，我们也能从上述log看出**type:"li"**

**null:**我们没有为元素定义任何的属性，但是我们可以添加像`className`属性，但是在上边的例子中我们没有那么做。因此我们传递了**null**值。

**React.js:**我们传入了文本作为元素的`children`，`children`不仅可以是文本还可以是其他元素。



在渲染期间，也就是`render`期间，`React`会将`React`元素转变成真实`DOM`。

```html
<li>React.js</li>
```

并且如果你想要给`li`元素添加一个类，可以像下边这样做：

```javascript
const element = React.createElement('li',{
	className:"list"},'React.js')
console.log(element)
```

<img src="C:\Users\86150\Desktop\读书笔记\public\hrarw\liClassName.jpg" style="zoom:67%;" />

从上图中我可以看到`props`中多出了一个`className`属性，证明我们添加成功了。

这里出现一个问题为什么不用`class`而是使用`className`？像

```javascript
<li class="list">React.js</li>
```

标签上使用的都是`class`。

答案就是`react`元素他所对标的`DOM`元素，而不是HTML，在`DOM API`中要为创建的`DOM`元素添加类的话也是使用`className`，这样二者就更加接近。

```javascript
const arr = document.createElement("div")
arr.className = "hello" 
```

### ReactDOM

当你尝试创建`React`元素之后，然后你想要在浏览器上看到它。但是浏览器不理解你创建的`React`元素。浏览器认为`React`元素只是一个普通的JS对象。`ReactDOM`就是将`React`元素渲染在浏览器上的一个“桥梁”。`ReactDOM`有许多有用的方法，但是我们最感兴趣的就是*render*方法。它需要两个**参数**来描述你需要**渲染什么元素**和元素需要**渲染在哪里**。

**例子**：在`DOM`中渲染上文所述的`li`元素

```javascript
const element = React.createElement('li',{
	class:"list"},'React.js')
const root = document.getElementById("root")
ReactDOM.render(element,root)
```

方法第二个参数是我们想要渲染`li`元素的位置，或者说我们想把`li`元素渲染成哪个其他元素的子节点，我们也可以使用`body`元素，但是这种行为是不推荐的。因为一般`document.body`元素经常被第三方库操作，我们直接操作`document.body`可能会出现一些不可预知的问题。

接下来我们使用`React`和`ReactDOM`进行构建之前的例子，然后渲染在浏览器页面上：

```javascript
//main.js
const mainElement = React.createElement('section',{
	className:'js-section'
},
React.createElement("h1",null, "JavaScript Libraries and Frameworks" ),
React.createElement("ul",{
	className:'list-lib-frameworks'
},
React.createElement("li",null,"React.js"),
React.createElement("li",null,"Vue.js"),
React.createElement("li",null,"Angular.js"),
React.createElement("li",null,"Node.js")
)
)
ReactDOM.render(mainElement,document.getElementById("root"))
```

从上边的例子可以看出如果给*render*方法提供超过三个参数，那么第三个以及第三个以后的都会被作为`children`。因此`React`可以创建一个`children`的数组，这些新创建的`children`元素被放进了`children`数组中。因此第三个参数我们可以使用数组，并且用数组可以简化我们的代码。

```javascript
const listOfLibAndFrameworks = ['React.js', 'Angular', 'Vue.js',
'Node.js', 'underscore.js'];
const mainElement = React.createElement('section',{
	className:'js-section'
},
React.createElement("h1",null, "JavaScript Libraries and Frameworks" ),
React.createElement("ul",{
	className:'list-lib-frameworks'
},
listOfLibAndFrameworks.map((item,index) => {
	return React.createElement('li',null,item)
})
)
)
ReactDOM.render(mainElement,document.getElementById("root"))
```

使用数组遍历节省了很多的代码量，但是在浏览器控制台中会发现如下警告：

<img src="C:\Users\86150\Desktop\读书笔记\public\hrarw\key.jpg" style="zoom:80%;" />

翻译过来就是每个数组子元素都应该有一个独一无二的`key`属性。那么我们在创建`li`元素的时候，通过给`React.createElement`方法第二个参数传递`key`属性就可以了：

```javascript
listOfLibAndFrameworks.map((element, index) => React.createElement(``'li'``,
    ``{ key: index }, element))
```

在`React`中，我们使用数组的时候，如果不加上`key`属性，就会在控制台发出警告。想要消除这个警告，我们就要加上上述的`key`属性。它用于`React`来识别唯一的`li`以便于重新渲染它，这样会让我们的代码更加高效，因此很推荐加上`key`值。

-----------------

总结：`React`在内存中创建`React element`（JS对象），然后`ReactDOM`将`React`创建好的视图元素渲染在浏览器页面上。

