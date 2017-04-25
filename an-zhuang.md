# 安装 {#安装}

---

## 试试Marko {#试试marko}

---

如果您只想在浏览器中尝试Marko，请转到我们的[Try Online](http://markojs.com/try-online)功能。您可以在浏览器中开发Marko应用程序。

---

## 新建一个App {#新建一个app}

---

如果你是刚开始使用marko，`marko-devtools`为你提供了一个入门程序帮助你快速开始学习。来一起开始吧：

```js
npm i marko-devtools -g
marko create hello-world
cd hello-world
npm install # or yarn
npm start
```

# 直接使用 {#直接使用}

---

## 安装 {#安装-1}

Marko编译器运行在Node.js上，并且可以使用npm安装：

```js
npm i marko --save
```

或者使用yarn

```js
yarn add marko
```

## 浏览器环境 {#浏览器环境}

假如我们有一个简单的页面，想要在浏览器中渲染：`hello.marko`

```js
<h1>Hello ${input.name}</h1>
```

首先，我们先创建一个`client.js`，用它将页面`require`进来并且渲染到`body`里：

```js
var helloComponent = require('./hello');
helloComponent.renderSync({name:'Marko'})
	.appendTo(document.body);
```

我们还将创建一个HTML页面来放置我们的应用：

```html
<!doctype html>
<html>
<head>
    <title>Marko Example</title>
</head>
<body>
 
</body>
</html>
```

现在，为了在浏览器中运行，我们需要打包这些文件。我们可以使用一个叫`lasso`的工具来帮我们完成，因此，我们先安装它（还有marko插件）：

```js
npm i -g lasso-cli
npm i --save lasso-marko
```

至此，我们可以打一个可以在浏览器中运行的包  
译者注：如果在此过程中出现`Cannot find module`报错,请将上面两个插件以`--save`方式安装

```js
lasso --main client.js --plugins lasso-marko --inject-into index.html
```

## 服务器环境 {#服务器环境}

### require Marko 页面 {#require-marko-页面}

Marko允许你像`require`标准的JavaScript包一样引入页面，看下面`server.js`的例子：  
**hello.marko**

```marko
<div>
    Hello ${input.name}!
</div>
```

**server.js**

```js
//以下一行是为了`.marko`文件而引入的Node.js扩展组件。
//在引用任何`*.marko`文件之前都应先将它`require`进来
require('marko/node-require');

var fs = require('fs');

//通过`require`一个`.marko`文件来加载Marko页面
var hello = require('./hello');
var out = fs.createWriteStream('hello.html',{ encoding:'utf8'});
hello.render({name:'Frank'},out);
```

使用Node.js`require`扩展完全是可选的，如果你不想使用它，你将需要使用`Marko DevTools`来预编译Marko模板

```js
marko compile hello.marko
```

这将在原始的模板同级目录下生成一个`hello.js`文件。生成的`.js`文件将是`Node.js`运行时加载的文件。

如果你只希望在开发环境中使用`require`扩展，你可以选择性的`require`它

```js
if(!process.env.NODE_ENV){
	require('marko/node-require');
}
```

### 在服务器上跑一个简单的页面 {#在服务器上跑一个简单的页面}

我们改写一下`server.js`以便于http服务器上运行一个页面  
**server.js**

```js
//这里可以require`.marko`文件
require('marko/node-require');

var http = require('http');
var hello = require('./hello');
var port = 8080;

http.createServer((req,res) => {
	res.setHeader('content-type','text/html');
	hello.render({name:'Marko'},res);
}).listen(port);
```

然后在`hello.marko`里面写一些内容：  
**hello.marko**

```marko
<h1>Hello ${input.name}</h1>
```

启动服务器（`node server.js`）然后打开浏览器访问`http://localhost:8080`,你就可以看到”Hello Marko”。

### 初始化服务端渲染组件 {#初始化服务端渲染组件}

Marko会在闭合标签`</body>`之前自动注入需要运行在浏览器中的组件列表。因此，需要在渲染输出中包含一个`<body>`标签。

但是，你仍然需要为你的页面打包CSS和JavaScript，并且嵌入`link`,`style`和`script`标签。幸运的是，`lasso`会帮你做这些繁琐的工作。

首先，安装`lasso`和`lasso-marko`:

```js
npm i --save lasso lasso-marko
```

然后，在你的页面或者`layout`模板中，添加`lasso-head`和`lasso-body`标签：  
**layout.marko**

```marko
<!doctype>
<html>
<head>
    <title>Hello world</title>
    <lasso-head/>
</head>
<body>
    <include(input.body)/>
    <lasso-body/>
</body>
</html>
```

最后，配置你的服务器来运行`lasso`生成的静态文件：  
**server.js**

```js
app.ues(require('lasso/middleware').serveStatic());
```



