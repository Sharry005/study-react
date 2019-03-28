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
   [redux简单示例](https://codesandbox.io/s/l5moll9moq) <br>
   [redux理论知识总结](https://codesandbox.io/s/l5moll9moq)
---------------------
#### 2. Hooks
   * useState 是和 function 一起使用，不能使用class.(之前 state 必须写在 class 中，useState 作者为了使 state 也可以同样使用在 function 中，所以创造了 useState)
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
      [无嵌套关系组件简单示例](https://codesandbox.io/s/lyz6o49wzq)
      ***
   ##### (3.5) 删除list中的某一项:
   * 引入 react-html-id 第三方库， 生成唯一的 id，采用 findIndex 找到当前的 index, 数组删除采用 splice(), 所有操作均在父组件中完成
   
   ##### (3.6) Fragment 用法 (React version >= 16):
   * 包裹元素，渲染时会 remove 掉自己，不在 html 中显示
   * 将 html 标签显示在页面上时， 可以使用 Fragment 来包裹
   * 需要在开头进行引入 import React, {Component, Fragment} from 'react'
   * [详细资料](https://vmo-fed.github.io/react/react-Fragments/)
