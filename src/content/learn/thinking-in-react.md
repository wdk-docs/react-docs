---
title: React中的思考
---

<Intro>

React可以改变你对设计和应用程序的看法。当您使用React构建用户界面时，您将首先将其分解为称为`组件`的部分。然后，您将描述每个组件的不同视觉状态。最后，您将把组件连接在一起，以便数据在其中流动。在本教程中，我们将指导您完成使用React构建可搜索产品数据表的思想过程。

</Intro>

## 从实物模型开始 {/*start-with-the-mockup*/}

假设您已经有一个JSON API和一个来自设计师的模型。

JSON API返回的一些数据如下:

```json
[
  { category: "Fruits", price: "$1", stocked: true, name: "Apple" },
  { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },
  { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },
  { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },
  { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },
  { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }
]
```

模型看起来是这样的:

<img src="/images/docs/s_thinking-in-react_ui.png" width="300" style={{margin: '0 auto'}} />

要在React中实现UI，通常需要遵循相同的五个步骤。

## 步骤1: 将UI分解为组件层次结构 {/*step-1-break-the-ui-into-a-component-hierarchy*/}

首先在实体模型中的每个零部件和子零部件周围绘制方框并命名它们。如果您与设计师合作，他们可能已经在其设计工具中命名了这些组件。问问他们！

根据您的背景，您可以考虑以不同的方式将设计拆分为多个组件:

* **编程**--使用相同的技术来决定是否应该创建一个新的函数或对象。 其中一种技术是[单一责任原则](https://en.wikipedia.org/wiki/Single_responsibility_principle), 也就是说，一个组件在理想情况下应该只做一件事。如果它最终生长，应该将其分解为更小的子组件。
* **CSS**--考虑一下您将为什么制作类选择器。 (然而，组件的颗粒度稍低。)
* **设计**--考虑如何组织设计的图层。

如果您的JSON结构良好，您经常会发现它自然地映射到UI的组件结构。这是因为UI和数据模型通常具有相同的信息架构，也就是说，具有相同的形状。将UI分离为多个组件，每个组件与数据模型的一部分相匹配。

这个屏幕上有五个组件:

<FullWidth>

<CodeDiagram flip>

<img src="/images/docs/s_thinking-in-react_ui_outline.png" width="500" style={{margin: '0 auto'}} />

1. `FilterableProductTable` (grey) 包含整个应用程序。
2. `SearchBar` (blue) 接收用户输入。
3. `ProductTable` (lavender) 根据用户输入显示和过滤列表。
4. `ProductCategoryRow` (green) 显示每个类别的标题。
5. `ProductRow`	(yellow) 为每个产品显示一行。

</CodeDiagram>

</FullWidth>

如果您查看`ProductTable`（淡紫色），您会发现表头（包含`Name`和`Price`标签）并不是它自己的组件。这是一个偏好问题，你可以选择任何一种方式。在本例中，它是`ProductTable`的一部分，因为它出现在`ProductTable`的列表中。但是，如果此标头变得复杂（例如，如果添加排序），则可以将其移动到自己的`ProductTableHeader`组件中。

既然您已经确定了模型中的组件，那么就将它们排列成一个层次结构。出现在实体模型中另一个组件中的组件应在层次结构中显示为子级：

* `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
        * `ProductCategoryRow`
        * `ProductRow`

## 步骤2: 在React中构建静态版本 {/*step-2-build-a-static-version-in-react*/}

现在您已经拥有了组件层次结构，是时候实现您的应用程序了。最直接的方法是构建一个版本，从数据模型中渲染UI，而不添加任何交互。。。然而通常先构建静态版本，然后添加交互性更容易。构建一个静态版本需要大量的打字而不需要思考，但添加交互性需要大量的思考而不需要大量的输入。

要构建呈现数据模型的应用程序的静态版本，您需要构建重用其他组件并使用[props](/learn/passing-props-to-a-component)传递数据的[components](/learn/your-first-component)。属性是将数据从父级传递到子级的一种方式。(如果你熟悉[状态](/learn/state-a-components-memory)的概念, 根本不要使用state来构建这个静态版本。状态仅用于交互，即随时间变化的数据。由于这是应用程序的静态版本，您不需要它。）

您可以从构建层次结构中较高的组件开始构建`自上而下` (如 `FilterableProductTable`) 或通过从下到上的组件工作实现`自下而上` (如 `ProductRow`). 在更简单的例子中，通常更容易自上而下，而在更大的项目中，更容易自下而上。

<Sandpack>

```jsx src/App.js
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
  return (
    <form>
      <input type="text" placeholder="Search..." />
      <label>
        <input type="checkbox" />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 10px;
}
td {
  padding: 2px;
  padding-right: 40px;
}
```

</Sandpack>

(如果这个代码看起来很吓人，请先浏览[快速入门](/learn/)！)

构建组件后，您将拥有一个可重用组件库，用于呈现数据模型。因为这是一个静态应用程序，所以组件只会返回JSX。 层次结构顶部的组件（`FilterableProductTable`）将把数据模型作为属性。 这被称为*单向数据流*，因为数据从顶层组件向下流到树的底部。

<Pitfall>

此时，您不应该使用任何状态值。这是下一步！

</Pitfall>

## 步骤3: 查找UI状态的最小但完整的表示形式 {/*step-3-find-the-minimal-but-complete-representation-of-ui-state*/}

为了使UI具有交互性，您需要允许用户更改您的底层数据模型。您将使用*state*。

将状态视为应用程序需要记住的最小变化数据集。构建状态最重要的原则是保持它[DRY（不要重复自己）](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) 找出应用程序所需状态的绝对最小表示形式，并按需计算其他所有内容。例如，如果您正在构建一个购物列表，则可以将项目存储为状态中的数组。如果您还想显示列表中的项数，请不要将项数存储为另一个状态值，而是读取数组的长度。

现在想想这个示例应用程序中的所有数据片段：

1. 原始产品列表
2. 用户输入的搜索文本
3. 复选框的值
4. 已筛选的产品列表

其中哪些是状态的？识别那些不是：

* 随着时间的推移，它是否**保持不变**？如果是这样，那就不是状态。
* 它是**通过属性从父母那里传来**的吗？如果是这样，那就不是状态。
* **你能计算一下吗** 基于组件中的现有状态或属性？ 如果是这样的话，那肯定不是状态！

剩下的可能是状态。

让我们一个接一个地再看一遍：

1. 产品的原始列表是**作为属性传递的，所以它不是状态** 
2. 搜索文本似乎是状态，因为它会随着时间的推移而变化，并且无法根据任何内容进行计算。
3. 复选框的值似乎是状态，因为它会随着时间的推移而变化，并且无法根据任何内容进行计算。
4. 过滤后的产品列表**不是状态，因为它可以通过获取原始产品列表并根据搜索文本和复选框的值进行过滤来计算**。

这意味着只有搜索文本和复选框的值才是状态！干得好！

<DeepDive>

#### Props vs State {/*props-vs-state*/}

React中有两种类型的`模型`数据：props和state。两者非常不同：

* [**属性**就像传递给函数的参数](/learn/passing-props-to-a-component)。它们允许父组件将数据传递给子组件并自定义其外观。例如，`窗体`可以将`颜色`属性传递给`按钮`。
* [**状态**就像一个组件的内存。](/learn/state-a-components-memory) 它允许组件跟踪一些信息，并根据交互对其进行更改。例如，`按钮`可能会跟踪`isHovered`状态。

属性和状态是不同的，但它们是协同作用的。父组件通常会将一些信息保持在状态（这样它就可以更改它），并将其`向下传递`给子组件作为它们的支柱。如果第一次阅读时仍然感觉模糊，那也没关系。它需要一点练习才能真正坚持下来！

</DeepDive>

## 步骤4: 确定您所在州应该居住的地方 {/*step-4-identify-where-your-state-should-live*/}

在确定应用程序的最小状态数据后，您需要确定哪个组件负责更改此状态，或者`拥有`该状态。记住：React使用单向数据流，将数据从父组件向下传递到子组件。可能还不清楚哪个组件应该拥有什么状态。如果你是这个概念的新手，这可能会很有挑战性，但你可以按照以下步骤来解决！

对于应用程序中的每个状态：

1. 识别基于该状态呈现某些内容的*每个*组件。
2. 找到它们最接近的公共父组件——层次结构中位于它们之上的组件。
3. 决定该州应该住在哪里:
    1. 通常，您可以将状态直接放入它们的公共父级中。
    2. 您也可以将状态放入其公共父级之上的某个组件中。
    3. 如果找不到拥有状态的组件，请创建一个仅用于保存状态的新组件，并将其添加到公共父组件上方的层次结构中的某个位置。

在上一步中，您在此应用程序中找到了两个状态：搜索输入文本和复选框的值。在这个例子中，它们总是一起出现，所以把它们放在同一个地方是有意义的。

现在让我们来了解一下我们为他们制定的战略:

1. **识别使用状态的组件:**
    * `ProductTable`需要根据该状态（搜索文本和复选框值）过滤产品列表。 
    * `SearchBar` 需要显示该状态（搜索文本和复选框值）。
1. **查找它们的共同父级:** 两个组件共享的第一个父组件是 `FilterableProductTable`.
2. **决定州的居住地**: 我们将在`FilterableProductTable`中保留筛选文本和检查状态值。

因此，状态值将存在于`FilterableProductTable`中。 

使用[`useState()`Hook](/reference/react/useState)将状态添加到组件中.钩子是一种特殊的功能，可以让你`钩入`React。在`FilterableProductTable`的顶部添加两个状态变量，并指定它们的初始状态：

```js
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);  
```

然后，作为属性通过 `filterText` 和 `inStockOnly` 到 `ProductTable` 和 `SearchBar` :

```js
<div>
  <SearchBar 
    filterText={filterText} 
    inStockOnly={inStockOnly} />
  <ProductTable 
    products={products}
    filterText={filterText}
    inStockOnly={inStockOnly} />
</div>
```

您可以开始了解应用程序的行为。在下面的沙盒代码中，将`filterText`初始值从`useState('')`编辑为`useState('fruit')`。您将看到搜索输入文本和表格更新：

<Sandpack>

```jsx src/App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} />
      <ProductTable 
        products={products}
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Search..."/>
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 5px;
}
td {
  padding: 2px;
}
```

</Sandpack>

请注意，编辑表单还不起作用。上面的沙盒中有一个控制台错误，解释了原因:

<ConsoleBlock level="error">

您为没有`onChange`处理程序的表单字段提供了`value`属性。这将呈现只读字段。

</ConsoleBlock>

在上面的沙盒中，`ProductTable`和`SearchBar`读取`filterText`和`inStockOnly`属性以呈现表、输入和复选框。例如，以下是`SearchBar`如何填充输入值：

```js {1,6}
function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Search..."/>
```

然而，您还没有添加任何代码来响应用户的操作，比如打字。这将是你的最后一步。


## 步骤5: 添加反向数据流 {/*step-5-add-inverse-data-flow*/}

当前，您的应用程序通过层次结构中向下流动的属性和状态进行正确渲染。但是，要根据用户输入更改状态，您需要支持数据以另一种方式流动：层次结构中的表单组件需要更新`FilterableProductTable`中的状态。

React使这个数据流显式，但它需要比双向数据绑定更多的类型。如果您尝试在上面的示例中键入或选中该框，您会看到React会忽略您的输入。这是故意的。通过书写 `<input value={filterText} />`, 您已将`input`的`value`属性设置为始终等于从`FilterableProductTable`传入的`filterText`状态。 由于从不设置`filterText`状态，因此输入永远不会更改。

您希望这样，每当用户更改表单输入时，状态都会更新以反映这些更改。该状态由`FilterableProductTable`所有，因此只有它才能调用`setFilterText`和`setInStockOnly`。要让`SearchBar`更新`FilterableProductTable`的状态，您需要将这些函数向下传递给`SearchBar'`：

```js {2,3,10,11}
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly}
        onFilterTextChange={setFilterText}
        onInStockOnlyChange={setInStockOnly} />
```

在`SearchBar`中，您将添加`onChange`事件处理程序，并从中设置父级状态：

```js {4,5,13,19}
function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input
        type="text"
        value={filterText}
        placeholder="Search..."
        onChange={(e) => onFilterTextChange(e.target.value)}
      />
      <label>
        <input
          type="checkbox"
          checked={inStockOnly}
          onChange={(e) => onInStockOnlyChange(e.target.checked)}
```

现在应用程序完全可以工作了！

<Sandpack>

```jsx src/App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} 
        onFilterTextChange={setFilterText} 
        onInStockOnlyChange={setInStockOnly} />
      <ProductTable 
        products={products} 
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} placeholder="Search..." 
        onChange={(e) => onFilterTextChange(e.target.value)} />
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} 
          onChange={(e) => onInStockOnlyChange(e.target.checked)} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding: 4px;
}
td {
  padding: 2px;
}
```

</Sandpack>

您可以在[添加交互性](/learn/adding-interactivity)部分了解有关处理事件和更新状态的所有信息。

## 从这里去哪里 {/*where-to-go-from-here*/}

这是一个非常简短的介绍，介绍了如何考虑使用React构建组件和应用程序。您可以立即[启动一个React项目](/learn/installation)，也可以[深入了解本教程中使用的所有语法](/learn/describing-the-ui)。
