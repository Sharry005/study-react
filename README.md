# study-react
self-study react and summarize

学习一个东西，从两方面入手：<br>
   (1) 这个东西是什么？ <br>
   (2) 这个东西解决了什么问题？ <br>
   
------------------


#### 1. Redux
   * Redux 是和 react 一点关系都没有的。
   * Redux 的实现原理是[订阅发布模式](https://vmo-fed.github.io/js-design-pattern/publish-subscribe-pattern/)。 
   * Redux 中分为 store, action, reducer 三部分。
   * Redux 是单项数据流， store 只能通过 reducer 获取， reducer 只能由 action 改变。
   * Redux 中的 API： subscribe, dispatch, getState<br>
   [sub / pub 简单示例---采用订阅发布模式](https://codesandbox.io/s/1z1n8y01lq) <br>
   [redux简单示例](https://codesandbox.io/s/l5moll9moq) <br>
   [redux理论知识总结](https://codesandbox.io/s/l5moll9moq)
---------------------
#### 2. Hooks
   * useState 是和 function 一起使用，不能使用class.(之前 state 必须写在 class 中，useState 作者为了使 state 也可以同样使用在 function 中，所以创造了 useState)
   <br>[简单示例](https://codesandbox.io/s/4qyqqpyvz4)
--------
#### 3. 基础知识 <br>
  ##### (3.1) 两种组件的定义方式： 
  * 使用class 定义
  
  ```
  class definedName extends React.Component {
     render () {
       return (
         <div>
           {this.props.name}
         </div>
       )
     }
   }
   ```
  
  * 使用 function 定义
  
  ```
  function definedName(props) {
     return (
       <div>
         {props.name}
       </div>
     )
   }
  ```
  
  - 二者区别：
  1. 有 state 的时候使用 class，纯函数使用 function
  2. 任何组件的 attribute 都是挂载在 props
     ***
  
  ##### (3.2) state 和 props： 
  * 组件无论是哪种方式声明，都不能修改自身的 props。所有 react 组件，都必须像保护纯函数一样，保护它们的 props 不被更改
  * props 来自于组件外部, state 来自于组件内部，发生变化的时候，会调 render 重新进行渲染
  * 改变 state, 只能使用 
  * setState 的同步异步问题：(暂时没有弄懂)
    1. 在 react 生命周期或者 React event hadnler 是异步
    2. 延时回调或者原生事件，不一定是异步，因为没有经过 react 的事物流
    ***
  ##### (3.3) 两种事件传参的调用方式：
  * 匿名函数的写法,此时不会立即执行，点击按钮的时候，匿名函数返回 this.changeName 立即执行<br>
    [匿名函数 和 bind 简单示例](https://codesandbox.io/s/3v71094m46)
  
  ```
  <button onClick={() => this.changeName('Sharry is happy')}>change state use anonymous function</button>
  <input type="text" onChange={(e) => this.changeNameWithInput(e) value={this.state.name}} />
  ```
  
  * bing 方法
  
  ```
  <button onClick={this.changeName.bind(this, 'Sharry is sad')}>change state use bind function</button>
  <input type="text" onChange={this.changeNameWithInput.bind(this)} value={this.state.name} />
  ```
  
  * 当 input 当中有 value 的时候，要么加 onChange 事件，要么设置 readOnly = {true}
      ***
  
  ##### (3.4) 组件通信的几种方式：
  * 父组件向子组件通信  
    react 数据流是单向的，父组件向子组件，只需通过 props 进行传递<br>
    [简单示例Child and Parent](https://codesandbox.io/s/pwyzjyrqwj)  
  * 子组件向父组件通信
    1. 利用回调函数
    2. 利用自定义事件机构
    [简单示例CallbackParent](https://codesandbox.io/s/pwyzjyrqwj)
  * 跨级组件通信
    1. 层层组件传递 props
       例如：组件A 和 组件B 之间要进行通信，先找到 组件A 和 组件B 公共的父组件C，A 先向 C 通信， C 通过 props 和 B 通信，此时 C 起到的就是一个中间件的作用
    2. 使用 context<br>
      (1) context 是一个全局变量，在任何地方都可以访问到它。将通信的信息放到 context 上，然后可以在其它组件中随意取到；<br>
      (2) React 官方不建议大量使用 context, 尽管它可以减少逐层传递。理由： <br>
          * 组件结构复杂时，我们并不知道 context 是从哪里传递过来
          * context 是一个全局变量，全库变量正是导致应用走向混乱的罪魁祸首
            [context简单示例 ListParent](https://codesandbox.io/s/pwyzjyrqwj)
   * 没有嵌套关系的组件通信
      * 使用自定义事件机制<br>
      (1) 在 ComponentDidMount 事件中，如果组件挂载完成，再订阅事件；在组件卸载的时候，在 ComponentWillUnmount 事件中取消事件的订阅<br>
      (2) 常用的发布 / 订阅模式举例，借用 Node.js Events 模块的浏览器版实现<br>
      (3) 自定义事件是典型的发布订阅模式，通过向事件上添加监听事件 和 触发事件来实现组件间的通信<br>
      [无嵌套关系组件简单示例](https://codesandbox.io/s/qo1pj79v4)
      ***
   ##### (3.5) 删除list中的某一项:
   * 引入 react-html-id 第三方库， 生成唯一的 id，采用 findIndex 找到当前的 index, 数组删除采用 splice(), 所有操作均在父组件中完成
   * [简单示例](https://codesandbox.io/s/52xr71x6xl)
      ***
   ##### (3.6) Fragment 用法 (React version >= 16):
   * 包裹元素，渲染时会 remove 掉自己，不在 html 中显示
   * 将 html 标签显示在页面上时， 可以使用 Fragment 来包裹
   * 需要在开头进行引入 import React, {Component, Fragment} from 'react'
   * [详细资料](https://vmo-fed.github.io/react/react-Fragments/)
       ***
   ##### (3.7) 生命周期:
   <br> 生命周期是一些函数，目的是在使得可以在正确的时机做正确的事，以下是 依次执行 的生命周期
   * constructor: <br>
         (1) 如果不初始化 state 或不进行方法绑定，则不需要为 react 组件实现构造函数<br>
         (2) 在 react 组件挂载之前,会调用它的构造函数。在为 React.Component 子类实现构造函数时，需要在前面加入 super(props)， 否则可能会出现 this.props 为定义的 bug <br>
         (3) 在 constructor 中不能调用 setState 方法, 只能进行 state 的初始化
         (4) 注意： 避免一个常见的错误，将 props 的值赋值给 state, 这是不允许的， 可以在render 中直接使用 this.props.color<br>
         
         ```
         constructor(props) {
          super(props);
          // 不要这样做
          this.state = { color: props.color };
         }
         ```
         
   * componentWillMount: (此生命周期方法即将过时，建议在代码中尽量避免使用)<br>
         (1) 在 constructor 之后执行，只执行一次   <br>
         (2) 是唯一一个在服务端渲染时执行 hook 的函数   <br>
         (3) 此时 state 和 props 已经初始化, 但是 component 还没有渲染 render  <br>
         (4) 可以在此处使用 setState, 举例： 假设 state 根据 props 值改变，在此 setstate 不会 re-render component, 依旧是 initial state  <br>
         (5) 全局的一些方法可以在此定义，例如： window / document
   * render: <br>
         (1) 在 componentWillMount 之后执行, state / props 改变时会重新渲染 render, 故此处不能使用 setState <br>
         (2) 假设我们有 component tree, render 时执行顺序是： sub component(顶级组件) ---->> children component 分别调用 constructor ---->> componentWillMount ---->> render, 直到 sub component finish render <br>
   * componentDidMount: <br>
         (1) 在 render 之后执行，只执行一次，如果有一个组件树，那么执行子组件的生命周期 从父组件的render开始，直到 子组件生命周期 执行完毕才会执行父组件的 componentDidMount <br>
         (2) 可以在这里调用 ajax 请求 <br>
         (3) 在这里创建发布订阅,这里是比较适合添加订阅的地方 (如果添加了订阅，要记得在 componentWillUnmount 中取消订阅) <br>
         (4) 在这里可以直接调用 setState()。它将触发渲染，但此渲染会发生在浏览器更新屏幕之前，如此保证了即使 render() 在两次调用的情况下，也不会让用户看到中间状态。但是谨慎在此处使用，因为会导致性能问题<br>
   * ***重新渲染 component 时的过程：<br>
        * (1) componentWillReceiveProps:(此生命周期方法即将过时, 建议在代码中尽量避免使用)<br>
              在这里可以看到即将给 render 的 state 和 props, 在这里不要改变 props 和 state 的值
        * (2) shouldComponentUpdate: <br>
              (2.1) 是否更新组件，返回 true， 则会更新，否则不更新 <br>
              (2.2) 首次渲染或使用 forceUpdate() 时不会调用该方法 <br>
              (2.3) 此方法仅作为"性能优化"的方式存在，不要企图依靠此方法来"阻止"渲染，因为这可能产生 bug <br>
              (2.4) 应该考虑使用 PureComponent 组件，而不是手动编写 shouldComponentUpdate(), PureComponent 会对 props 和 state 进行浅层比较，并减少了跳过必要更新的可能性<br>
         
         ```
         shouldComponentUpdate(nextProps, nextState) {
             return true;
         }
         ```       
      
        * (3) componentWillUpdate: 如果想定义一些基于 state 和 props 的变量，可以在此处定义，这里不可以使用 setState, 会重复 re-render 的过程，会陷入死循环
        * (4) render
        * (5) componentDidUpdate: 创建第三方 UI 库的 elements
        
        ```
        componentDidUpdate(nextProps, nextState) {}
        ```      
        
   * componentWillUnmount: 销毁的时候执行，可以在这里清除一些计数器<br>
     [简单示例](https://codesandbox.io/s/n7q92j91nm)
        ***
   ##### (3.8) Pure Components:
   <br> 了解 Pure Component 在 re-render 过程中的作用
   * setState 有一个特点：只要调用了这个方法，就会 re-render component, 它不会检查 value 的值有没有改变，为了防止 value 一样的时候还会重新渲染 render，可以采用以下方法：
     * (1) shouldComponentUpdate
     * (2) PureComponent: 浅拷贝，一般只适用于一个 叶子组件 的情况，如果 setState 传递多维数组，一维数组相同时默认为组件没有变化，所以一般不建议使用 PureComponent (目前没有理解，这块知识欠缺)
