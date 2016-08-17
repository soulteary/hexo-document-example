从技术上来说，在 ```cmis``` 中，我们可以仅使用标准的 ```html元素``` 就可以构建出任何页面，但是在实践中，我们通常都会扩展 ```html元素```，以达到代码复用和减小页面复杂度的目的。这些扩展的 ```html元素``` 其本质上就是我们通常所说的 ```Component```。


## 扩展 HTML 元素

```cmis``` 支持使用 [React](https://facebook.github.io/react/) 技术构建的组件来扩展页面中可使用的 ```html元素```。所以，在自定义HTML元素之前，你需要先了解 ```React``` 技术。

## 创建React Component

```cmis``` 内置了对 [React](https://facebook.github.io/react/) 的支持，所以无需再安装 [React](https://facebook.github.io/react/) 相关编译工具。

### 配置 React Component 源码路径

要创建并使用 [React](https://facebook.github.io/react/) Component， 我们需要通过 ```~/cmis.config.js```<sup>(注1)</sup> 文件告知 ```cmis``` 组件源码所在路径。

> 注1：```cmis.config.js``` 是 ```cmis``` 项目的配置文件，其位置是在项目根路径下，该文件由用户手动创建或者通过 ```cmis init``` 命令自动生成。

编辑 ```~/cmis.config.js``` 文件：

```
/*
 * 该文件内容的语法和API必须遵守 Node.js 的要求
 */
var path = require('path');

module.exports = {
	// React Component 配置
	modules: {
		/*
		 * key: 引入 React Component 后的命名空间，
		 *		要求必须是全大写，只能包含字母和数字，且必须以字母开头。
		 *	 
		 * value: 路径字符串，指定了 React Component 源码位置。
		 */
		'DEMO': path.join("__dirname", "components")
	}
};
```

### React Component 源码目录结构

```cmis``` 以 **目录** 的方式组织单个 React Component，每一个这样的目录下包含一个 ```index.js``` 文件作为该 Component 的主文件，需要 **导出** 一个 React Component。如果 Component 还包含其他资源文件，则也应该放在同一目录下。

根据上一节中 React Component 源码路径的配置，一个可能的组件目录结构看起来像这样：

```
~/components
	|- hello-world 			// Component 实现目录
		|- index.js 			// Component 实现主文件
```

### 编写React Component

编辑 ```~/components/hello-world/index.js```，加入以下内容<sup>(注1)</sup>：

```javascript
/*
 * 一个简单 React Component 的实现
 */
 
 import React, {Component} from 'react';
 
 // 这里必须以默认方式导出class。
 // 这里定义的类名并不是最终在页面中使用组件时的名称
 export default class HelloWorld extends Component {
 	render() {
 		return (
 			<div>Hello World!</div>
 		);
 	}
 }
 
```

> 注1：本例仅供参考，有关 React Component 的细节，请参考其文档：[https://facebook.github.io/react/](https://facebook.github.io/react/)

### React Component 名称规则

```cmis``` 会对目录名做一次转换，转换后的名称就是在页面中使用该 React Component 的名称，具体的转换规则如下：

1. 将目录名称的首字母转换成大写。
2. 如果名称由多个单词组成，且由 ```-``` 符号分割，则去掉 ```-``` 分隔符，且把每个单词的首字母进行大写。

根据这一规则，上一节示例中的 ```hello-world``` Compoennt，其名称为 ```HelloWorld```。

### 使用 React Component

在页面中，可以通过 ```<命名空间.组件名称/>``` 的方式来使用 React Component。

编辑 ```~/pages/index/index.html``` 文件，输入以下内容：

```html
<%@route path="/"%>

<DEMO.HelloWorld/>
```

### 预览效果

执行编译

```
cmis build
```

如果未启动开发服务器，则启动：

```
cmis start
```

打开浏览器，URL为：

```
http://localhost:3000
```

页面中会显示 ```Hello World!```。