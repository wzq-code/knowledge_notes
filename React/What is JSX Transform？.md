首先明确：

**React和JSX是两个独立的东西，只不过是这二者通常一起使用。**

---------------

浏览器无法理解JSX,因此大多数React使用者依赖于像Babel或者TypeScript将JSX转换为常规的JavaScript。很多脚手架工具都内置了JSX转换器，如`Create React App`或者`Next.js`等。

#### 新的JSX转换器有什么不同？

当你使用JSX的时候，编译转化器会将JSX转换成浏览器能理解的React函数调用（React function calls）。旧的转换器会将JSX转换成React.createElement(...)调用。

**例子：**

```jsx
import React from 'react'
function App() {
	return <h1>Hello World</h1>
}
```

在底层，旧的JSX转换器会将上述类标签代码转换为常规的JavaScript函数：

```jsx
import React from 'react';

function App() {
  return React.createElement('h1', null, 'Hello world');
}
```

然而这样的转换并不完美：

1. 因为JSX是被编译成`React.createElement`，所以要使用JSX的话，就需要将React引入到作用域中。即：

   ```javascript
   import React from 'react'
   ```

2. 由于`React.createElement`的限制，做不了一些性能上的提升和优化。

为了解决这些问题，React17在`React package`中引入了两个新的入口点，这两个入口点是仅仅被编译器使用，像babel和typescript。新的转换器不会将JSX转换为`React.createElement`，而是从`React package`中的两个入口点中自动引入特殊函数并调用它们。

**例子：**

```javascript
function App() {
	return <h1>Hello World</h1>
}
```

下边是新的转换器编译的代码：

```javascript
import {jsx as _jsx} from 'react/jsx-runtime';

function App() {
    return _jsx('h1',{children:'Hello World'});
}
```

现在我们不需要引入React就可以使用JSX了。但是我们仍然需要安装React，目的是使用Hooks或者其他React提供的包。

