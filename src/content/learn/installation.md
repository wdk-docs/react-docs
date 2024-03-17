---
title: 安装
---

<Intro>

React从一开始就被设计用于逐步采用。您可以根据需要使用尽可能少或尽可能多的React。无论你是想体验一下React，还是想在HTML页面上添加一些交互性，或者启动一个复杂的React应用程序，本节都将帮助你入门。

</Intro>

<YouWillLearn isChapter={true}>

* [如何启动新的React项目](/learn/start-a-new-react-project)
* [如何将React添加到现有项目中](/learn/add-react-to-an-existing-project)
* [如何设置编辑器](/learn/editor-setup)
* [如何安装React Developer Tools](/learn/react-developer-tools)

</YouWillLearn>

## 试用 React {/*try-react*/}

你不需要安装任何东西来玩React。尝试编辑此沙盒！

<Sandpack>

```js
function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}

export default function App() {
  return <Greeting name="world" />
}
```

</Sandpack>

您可以直接编辑它，也可以按右上角的`叉子`按钮在新选项卡中打开它。

React文档中的大多数页面都包含这样的沙盒。 
除了React文档之外，还有许多在线沙盒支持React: 例如, [CodeSandbox](https://codesandbox.io/s/new), [StackBlitz](https://stackblitz.com/fork/react), 或 [CodePen.](https://codepen.io/pen?&editors=0010&layout=left&prefill_data_id=3f4569d1-1b11-4bce-bd46-89090eed5ddb)

### 尝试本地React {/*try-react-locally*/}

要在您的计算机上尝试本地React，请[下载此HTML页面。](https://gist.githubusercontent.com/gaearon/0275b1e1518599bbeafcde4722e79ed1/raw/db72dcbf3384ee1708c4a07d3be79860db04bff0/example.html) 在编辑器和浏览器中打开它！

## 启动一个新的React项目 {/*start-a-new-react-project*/}

如果你想完全使用React构建一个应用程序或网站，[启动一个新的React项目。](/learn/start-a-new-react-project)

## 将React添加到现有项目 {/*add-react-to-an-existing-project*/}

如果想尝试在现有应用程序或网站中使用React，请[将React添加到现有项目中。](/learn/add-react-to-an-existing-project)

## 接下来的步骤 {/*next-steps*/}

前往[快速入门](/learn)指南，了解您每天都会遇到的最重要的React概念。

