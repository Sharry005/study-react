# study-react
self-study react and summarize

1. Redux
   * Redux 是和 react 一点关系都没有的。
   * Redux 的实现原理是[订阅发布模式](https://vmo-fed.github.io/js-design-pattern/publish-subscribe-pattern/)。 
   * Redux 中分为 store, action, reducer 三部分。
   * Redux 是单项数据流， store 只能通过 reducer 获取， reducer 只能由 action 改变。
   * Redux 中的 API： subscribe, dispatch, getState<br>
   [redux简单示例](https://codesandbox.io/s/l5moll9moq)
   [redux理论知识总结](https://codesandbox.io/s/l5moll9moq)
---------------------
2. Hooks
   * useState 是和 function 一起使用，不能使用class.(之前 state 必须写在 class 中，useState 作者为了使 state 也可以同样使用在 function 中，所以创造了 useState)
