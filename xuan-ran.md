\#渲染

\*\*\*

在渲染Marko视图之前，你需要先\`require\`它。

\*\*example.js\*\*

\`\`\`

var fancyButton = require\('./components/fancy-button'\);

\`\`\`

&gt;\*\*注意：\*\*如果你在Node.js环境下，则需要启用\`require\`扩展才能require\`.marko\`文件，或者你可以使用Marko DevTools来预编译所有模板。如果你在浏览器环境下使用，则需要使用一个打包器，例如\`lasso\`,\`webpack\`,\`browserify\`,\`rollup\`等。



一旦你写好了视图，就可以向它传入数据并且渲染页面：

\*\*example.js\*\*

\`\`\`

var button = require\('./component/fancy-button'\);

var html = button.renderToString\({ label:'Click me!'}\);



console.log\(html\);

\`\`\`

如果\`fancy-button.marko\`像这样，输入的数据在视图中就可以使用了：

\*\*./components/fancy-button.marko\*\*

\`\`\`

&lt;button&gt;${input.label}&lt;/button&gt;

\`\`\`

输出的HTML将会是这样子：

\`\`\`

&lt;button&gt;Click me!&lt;/button&gt;

\`\`\`



\#\#渲染方法

上面的例子我们使用\`render ToString\`方法渲染视图，但实际上有很多不同的方法可以用来渲染。



许多渲染方法返回了一个\`RenderResult\`,这是一个带有helper方法的对象，和渲染输出的内容一起使用。



\#\#\#\`renderSync\(input\)\`

\|参数\|类型\|描述\| 

\|-\|-\|-\|

\|\`input\`\|\`object\`\|用来渲染页面的输入数据\|

\|return value\|\`RenderResult\`\|渲染结果\|



使用\`renderSync\`强制完全同步渲染。如果一个标签试图异步运行，将会抛出错误。

\`\`\`

var view = require\('./view'\);

var result = view.renderSync\({}\);



result.appendTo\(document.body\);

\`\`\`



\#\#\#\`render\(input\)\`

\|参数\|类型\|描述\|

\| - \| - \| - \|

\|\`input\`\|\`Object\`\|用来渲染页面的输入数据\|

\|return value \|\`AsyncStream/AsyncVDOMBuilder\`\|异步渲染结果\|



\`render\`方法返回了一个异步的输出用来在服务端生成HTML或者在浏览器端生成一个虚拟DOM。在任何一种情况下，异步输出都具有遵循Promise/A+ 规范的then方法，因此它可以像Promise一样被使用。这个Promise解析成了一个\`RenderResult\`。

\`\`\`

var view = require\('./view'\);

var resultPromise = view.render\({}\);



resultPromise.then\(\(result\) =&gt; {

	result.appendTo\(document.body\);

}\)

\`\`\` 



\#\#\#\`render\(input,callback\)\`

\|参数\|类型\|描述\|

\|-\|-\|-\|

\|\`input\`\|\`Object\`\|用来渲染页面的输入数据\|

\|\`callback\`\|\`Function\`\|当渲染完成时的一个回调函数\|

\|callback value\|\`RenderResult\`\|渲染结果\|

\|返回值\|\`AsyncStream/AsyncVDOMBuilder\`\|异步渲染结果\|



\`\`\`

var view = require\('./view'\);

view.render\({},\(err,result\) =&gt; {

	result.appendTo\(document.body\);

}\)

\`\`\`



\#\#\#\`render\(input,stream\)\`

\|参数\|类型\|描述\|

\|-\|-\|-\|

\|\`input\`\|\`Object\`\|用来渲染页面的输入数据\|

\|\`stream\`\|\`WritableStream\`\|一个可写的流\|

\|返回值\|\`AsyncStream/AsyncVDOMBuilder\`\|异步渲染的\`out\`\|



HTML被写入输出的流中。

\`\`\`

var http = require\('http'\);

var view = require\('./view'\);

http.createServer\(\(req,res\) = &gt; {

	res.setHeader\('content-type','text/html'\);

	view.render\({},res\);

}\)

\`\`\`

\#\#\#\`render\(input,out\)\`

参数\|类型\|描述

-\|-\|-

\`input\`\|\`Object\`\|用来渲染页面的输入数据

\`out\`\|\`AsyncvStream/AsyncVDOMBuilder\`\|用来渲染的异步\`out\`

返回值\|\`AsyncStream/AsyncVDOMBuilder\`\|被传输的\`out\`





