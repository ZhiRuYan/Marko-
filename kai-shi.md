\#开始

\*\*\*

最简单的开始使用Marko的方法是使用\[在线环境\]\(http://markojs.com/try-online/\)。你只需要在新标签页中打开它即刻。如果你更喜欢本地开发，请查看安装页面。

\#\#Hello World

\*\*\*

 Marko可以让你使用像HTML那样的语法来轻松地表示UI

 \*\*hello.marko\*\* 

\`\`\`

 &lt;h1&gt;Hello World&lt;/h1&gt;

\`\`\`

 事实上，Marko跟HTML非常像，你甚至可以用它来替代模板语言，例如pug：

 \*\*template.marko\*\*

\`\`\`

&lt;!doctype html&gt;

html

    head

        title -- Hello World

    body

        h1 -- Hello World

\`\`\`

然而，Marko不仅是一个模板语言。它也是一个UI库，允许你将你的应用分解成独立的组件，而且描述了页面是如何随时间和用户操作而变化的。



在浏览器中，当有关UI的数据发生改变时，Marko将自动高效地更新DOM。



\#\#一个简单的组件

\*\*\*

假设我们有一个\`&lt;button&gt;\`，我们希望给它添件一些点击事件：

\*\*button.marko\*\*

\`\`\`

&lt;button&gt;Click me!&lt;/button&gt;

\`\`\`

Marko非常容易实现这个，它允许你在\`.marko\`视图中给组件定义一个\`class\`，并使用\`on\`属性调用该方法。

\`\`\`

class {

	sayHi\(\){

		alert\('Hi'\);

	}

}



&lt;button on-click\(sayHi\)&gt;Click me!&lt;/button&gt;

\`\`\`

\#\#添加 State

如何更新UI来响应操作呢?Marko的状态化组件让这个变得很容易。你需要做的是在你的组件中设置\`this.state\`。这使得你的视图可以使用新的\`state\`变量。当\`this.state\`中的值改变时，视图将自动重新渲染并且只更新DOM改变的部分。

\*\*counter.marko\*\*

\`\`\`

class {

	onCreate\(\){

		this.state = {

			count:0

		}

	}

	increment\(\){

		this.state.count++;

	}

}

&lt;div&gt;The current count is ${state.count}&lt;/div&gt;

&lt;button on-click\('increment'\)&gt;Click me!&lt;/button&gt;

\`\`\`

