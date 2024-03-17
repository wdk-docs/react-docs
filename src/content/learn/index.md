---
title: 快速入门
---

<Intro>

欢迎使用React文档！本页将向您介绍您将每天使用的80%的React概念。

</Intro>

<YouWillLearn>

- 如何创建和嵌套组件
- 如何添加标记和样式
- 如何显示数据
- 如何渲染条件和列表
- 如何响应事件和更新屏幕
- 如何在组件之间共享数据

</YouWillLearn>

## 创建和嵌套零部件 {/*components*/}

React应用程序是由*组件*组成的。
组件是UI（用户界面）的一部分，它有自己的逻辑和外观。
一个组件可以小到一个按钮，也可以大到整个页面。

React组件是返回标记的JavaScript函数：

```js
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
```

现在您已经声明了`MyButton`，可以将其嵌套到另一个组件中：

```js {5}
export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

请注意，`<MyButton />`以大写字母开头。这就是你知道它是React组件的原因。React组件名称必须始终以大写字母开头，而HTML标记必须小写。

看看结果:

<Sandpack>

```js
function MyButton() {
  return (
    <button>
      I'm a button
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

</Sandpack>

`export default`关键字指定文件中的主要组件。如果您不熟悉某些JavaScript语法， [MDN](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) 和 [javascript.info](https://javascript.info/import-export) 有很好的参考资料.

## 使用JSX编写标记 {/*writing-markup-with-jsx*/}

您在上面看到的标记语法称为*JSX*。它是可选的，但大多数React项目使用JSX是为了方便。所有[我们建议用于本地开发的工具](/learn/installation)都支持开箱即用的JSX。

JSX比HTML更严格。您必须关闭诸如`<br />`之类的标记。 
您的组件也不能返回多个JSX标记。 
你必须将它们包装成一个共享的父级, 像一个 `<div>...</div>` 或者空的 `<>...</>` 包装物:

```js {3,6}
function AboutPage() {
  return (
    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
    </>
  );
}
```

如果有很多HTML要移植到JSX，可以使用[在线转换器](https://transform.tools/html-to-jsx)

## 添加样式 {/*adding-styles*/}

在React中，您可以使用`className`指定一个CSS类。 
它的工作方式与HTML[`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class)属性相同：

```js
<img className="avatar" />
```

然后在一个单独的CSS文件中为其编写CSS规则:

```css
/* In your CSS */
.avatar {
  border-radius: 50%;
}
```

React没有规定如何添加CSS文件。 
在最简单的情况下，您将在HTML中添加一个[`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link)标记。
如果您使用构建工具或框架，请参阅其文档，了解如何将CSS文件添加到项目中。

## 显示数据 {/*displaying-data*/}

JSX允许您将标记放入JavaScript中。花括号可以让您"escape back"到JavaScript中，这样您就可以从代码中嵌入一些变量并将其显示给用户。例如，这将显示`user.name`：

```js {3}
return (
  <h1>
    {user.name}
  </h1>
);
```

您也可以从JSX属性`转义到JavaScript`，但必须使用大括号*而不是*引号。
例如, `className="avatar"` 将`avatar`字符串作为CSS类传递, 但是 `src={user.imageUrl}`读取JavaScript`user.imageUrl` 可变值, 然后将该值作为`src`属性传递:

```js {3,4}
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

您也可以在JSX大括号中放入更复杂的表达式，例如[字符串串联](https://javascript.info/operators#string-concatenation-with-binary):

<Sandpack>

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

```css
.avatar {
  border-radius: 50%;
}

.large {
  border: 4px solid gold;
}
```

</Sandpack>

在上面的例子中, `style={{}}` 不是一种特殊语法, 而是`style={ }`JSX大括号内的常规`{}`对象。 
当样式依赖于JavaScript变量时，可以使用`style`属性。

## 条件呈现 {/*conditional-rendering*/}

在React中，写条件没有特殊的语法。相反，您将使用与编写常规JavaScript代码时相同的技术。
例如，您可以使用[`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)声明有条件地包括JSX：

```js
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

如果您喜欢更紧凑的代码，可以使用[条件`?`运算符。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)与`if`不同，它在JSX内部工作：

```js
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

当您不需要`else`分支时，也可以使用较短的[必然的`&&`语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation):

```js
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

所有这些方法也适用于有条件地指定属性。如果您不熟悉某些JavaScript语法，可以从始终使用`if...else`开始。

## 渲染列表 {/*rendering-lists*/}

您将依赖JavaScript功能，如 [`for` loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) 和 [array `map()` 函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 渲染组件列表.

例如，假设您有一系列产品：

```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
```

在组件内部，使用`map()`函数将产品数组转换为`<li>`项数组：

```js
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

请注意`<li>`是如何具有`key`属性的。对于列表中的每个项目，您应该传递一个字符串或数字，该字符串或数字在其同级项目中唯一标识该项目。通常，密钥应该来自您的数据，例如数据库ID。React使用您的密钥来了解以后插入、删除或重新排序项目时发生了什么。

<Sandpack>

```js
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

</Sandpack>

## 对事件的响应 {/*responding-to-events*/}

您可以通过在组件内声明*eventHandler*函数来响应事件：

```js {2-4,7}
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

请注意`onClick={handleClick}`末尾没有括号！不要调用事件处理程序函数：您只需要*将其向下传递*。当用户单击按钮时，React将调用您的事件处理程序。

## 更新屏幕 {/*updating-the-screen*/}

通常，你会希望你的组件`记住`一些信息并显示出来。例如，也许你想计算一个按钮被点击的次数。为此，请将*state*添加到组件中。

第一, 从React导入[`useState`](/reference/react/useState):

```js
import { useState } from 'react';
```

现在，您可以在组件中声明一个*状态变量*：

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

您将从`useState`获得两样东西: 当前状态 (`count`), 以及让您更新它的功能 (`setCount`). 你可以给他们起任何名字，但惯例是写 `[something, setSomething]`.

第一次显示按钮, `count` 值是 `0` 因为你传输了 `0` 到 `useState()`.当您想要更改状态时, 呼叫 `setCount()` 并将新值传递给它。 单击此按钮将增加计数器:

```js {5}
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

React将再次调用您的组件函数。 这次, `count` 将是 `1`. 那么它将是`2`。等等

如果多次渲染同一个组件，每个组件都将获得自己的状态。分别单击每个按钮：

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

注意每个按钮是如何*记得*自己的`count`状态而不影响其他按钮的。

## 使用挂钩 {/*using-hooks*/}

以`use`开头的函数称为*Hooks*。 `useState`是React提供的内置Hook。您可以在[API参考](/reference/react) 中找到其他内置挂钩。您也可以通过组合现有的Hook来编写自己的Hook。

挂钩比其他功能更具限制性。您只能在组件（或其他Hook）的*顶部调用Hooks*。 如果要在条件或循环中使用`useState`，请提取一个新组件并将其放在那里。

## 组件之间共享数据 {/*sharing-data-between-components*/}

在前面的例子中，每个`MyButton`都有自己独立的`count`，当单击每个按钮时，只有单击按钮的`count`发生了变化：

<DiagramGroup>

<Diagram name="sharing_data_child" height={367} width={407} alt="Diagram showing a tree of three components, one parent labeled MyApp and two children labeled MyButton. Both MyButton components contain a count with value zero.">

开始, 每个 `MyButton` 的 `count` 状态是 `0`

</Diagram>

<Diagram name="sharing_data_child_clicked" height={367} width={407} alt="The same diagram as the previous, with the count of the first child MyButton component highlighted indicating a click with the count value incremented to one. The second MyButton component still contains value zero." >

第一个 `MyButton` 更新它的 `count` 为 `1`

</Diagram>

</DiagramGroup>

然而，您通常需要组件来*共享数据并始终一起更新*。

要使两个`MyButton`组件显示相同的`count`并一起更新，您需要将状态从单个按钮`向上`移动到包含所有按钮的最近组件。

在本例中 `MyApp`:

<DiagramGroup>

<Diagram name="sharing_data_parent" height={385} width={410} alt="Diagram showing a tree of three components, one parent labeled MyApp and two children labeled MyButton. MyApp contains a count value of zero which is passed down to both of the MyButton components, which also show value zero." >

最初，`MyApp`的`count`状态为`0`，并传递给两个子级

</Diagram>

<Diagram name="sharing_data_parent_clicked" height={385} width={410} alt="The same diagram as the previous, with the count of the parent MyApp component highlighted indicating a click with the value incremented to one. The flow to both of the children MyButton components is also highlighted, and the count value in each child is set to one indicating the value was passed down." >

单击后，`MyApp`会将其`count`状态更新为`1`，并将其传递给两个子项

</Diagram>

</DiagramGroup>

现在，当您单击任一按钮时，`MyApp`中的`count`将更改，这将更改`MyButton`中的两个计数。以下是如何用代码表达这一点。

首先，*将状态*从`MyButton`上移到`MyApp`：

```js {2-6,18}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>单独更新的计数器</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... we're moving code from here ...
}

```

然后，*将状态*从`MyApp`向下传递到每个`MyButton`，以及共享的单击处理程序。您可以使用JSX大括号将信息传递给`MyButton`，就像以前使用内置标记（如`<img>`）所做的那样：

```js {11-12}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>一起更新的计数器</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

你这样传递下来的信息叫做 _props_。现在，`MyApp`组件包含`count`状态和`handleClick`事件处理程序，并*将它们作为参数*传递给每个按钮。

最后，将`MyButton`更改为*read*您从其父组件传递的参数：

```js {1,3}
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

单击该按钮时，`onClick`处理程序将启动。 
每个按钮的`onClick`参数都设置为`MyApp`中的`handleClick`函数，因此它内部的代码会运行。 
该代码调用`setCount(count + 1)`，递增`count`状态变量。
新的`count`值作为参数传递给每个按钮，因此它们都显示新值。
这被称为`提升状态向上`。
通过向上移动状态，您已经在组件之间共享了它。

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>一起更新的计数器</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

## 接下来的步骤 {/*next-steps*/}

到目前为止，您已经了解了如何编写React代码的基本知识！

查看[教程](/learn/tutorial-tic-tac-toe)，将其付诸实践，并使用React构建您的第一个迷你应用程序。
