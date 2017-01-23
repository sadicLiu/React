## 0100_Basic_HTML_Template:React中html文件的基本模板
- 最后一个 `<script>` 标签的 type 属性为 `text/babel` 。这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 `type="text/babel"`
- 上面代码一共用了三个库： `react.js` 、`react-dom.js` 和 `Browser.js` ，它们必须首先加载。其中，`react.js` 是 React 的核心库，`react-dom.js` 是提供与 DOM 相关的功能，`Browser.js` 的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成
- `$ babel src --out-dir build`  
上面命令可以将 src 子目录的 js 文件进行语法转换，转码后的文件全部放在 build 子目录。

## 0200_ReactDOM.render
- `ReactDOM.render` 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点

## 0300_JSX
- 上一节的代码， HTML 语言直接写在 JavaScript 语言之中，不加任何引号，这就是 JSX 的语法，它允许 HTML 与 JavaScript 的混写
- 上面代码体现了 JSX 的基本语法规则：遇到 HTML 标签（以 `<` 开头），就用 HTML 规则解析；遇到代码块（以 `{` 开头），就用 JavaScript 规则解析。上面代码的运行结果如下。

## 0400_Variable_In_JSX
- JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员

## 0500_Component
- React 允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件。`React.createClass` 方法就用于生成一个组件类
- 变量 `HelloMessage` 就是一个组件类。模板插入 `<HelloMessage />` 时，会自动生成 `HelloMessage` 的一个实例（下文的"组件"都指组件类的实例）。所有组件类都必须有自己的 render 方法，用于输出组件
- 组件类的第一个字母必须大写，否则会报错，比如HelloMessage不能写成helloMessage。另外，组件类只能包含一个顶层标签，否则也会报错
- 组件的属性可以在组件类的 `this.props` 对象上获取，比如 `name` 属性就可以通过 `this.props.name` 读取
- 添加组件属性，有一个地方需要注意，就是 `class` 属性需要写成 `className` ，`for` 属性需要写成 `htmlFor` ，这是因为 `class` 和 `for` 是 JavaScript 的保留字

## 0501_Nested_Component
- React自定义组件可以相互嵌套

## 0600_this.props.children
- `this.props` 对象的属性与组件的属性一一对应，但是有一个例外，就是 `this.props.children` 属性。它表示组件的所有子节点
- 这里需要注意， `this.props.children` 的值有三种可能：如果当前组件没有子节点，它就是 `undefined` ;如果有一个子节点，数据类型是 `object` ；如果有多个子节点，数据类型就是 `array` 。所以，处理 `this.props.children` 的时候要小心
- React 提供一个工具方法 `React.Children` 来处理 `this.props.children` 。我们可以用 `React.Children.map` 来遍历子节点，而不用担心 `this.props.children` 的数据类型是 `undefined` 还是 `object`

## 0700_Component_propTypes
- 组件的属性可以接受任意值，字符串、对象、函数等等都可以。有时，我们需要一种机制，验证别人使用组件时，提供的参数是否符合要求
- 组件类的`PropTypes`属性，就是用来验证组件实例的属性是否符合要求
- `getDefaultProps` 方法可以用来设置组件属性的默认值

## 0800_getRealDOM
- 组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM （virtual DOM）。只有当它插入文档以后，才会变成真实的 DOM 。根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 DOM diff ，它可以极大提高网页的性能表现
- 但是，有时需要从组件获取真实 DOM 的节点，这时就要用到 ref 属性
- 需要注意的是，由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入文档以后，才能使用这个属性，否则会报错

## 0900_State
- 组件免不了要与用户互动，React 的一大创新，就是将组件看成是一个状态机，一开始有一个初始状态，然后用户互动，导致状态变化，从而触发重新渲染 UI
- `getInitialState` 方法用于定义初始状态，也就是一个对象，这个对象可以通过 `this.state` 属性读取。当用户点击组件，导致状态变化，`this.setState` 方法就修改状态值，每次修改以后，自动调用 `this.render` 方法，再次渲染组件。
- 由于 `this.props` 和 `this.state` 都用于描述组件的特性，可能会产生混淆。一个简单的区分方法是，`this.props` 表示那些一旦定义，就不再改变的特性，而 `this.state` 是会随着用户互动而产生变化的特性

## 1000_Form
- `textarea` 元素、`select`元素、`radio`元素都属于这种情况
- 用户在表单填入的内容，属于用户跟组件的互动，所以不能用 `this.props` 读取，应该用 `this.state`

## 1100_Component_Lifecycle
- 组件的生命周期分成三个状态：
  1. Mounting：已插入真实 DOM
  2. Updating：正在被重新渲染
  3. Unmounting：已移出真实 DOM
- React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数
```
  componentWillMount()
  componentDidMount()
  componentWillUpdate(object nextProps, object nextState)
  componentDidUpdate(object prevProps, object prevState)
  componentWillUnmount()
```
- 此外，React 还提供两种特殊状态的处理函数
```
componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用
```
