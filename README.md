# study-react
self-study react and summarize

1. Redux
   * Redux 是和 react 一点关系都没有的。
   * Redux 的实现原理是[订阅发布模式](https://vmo-fed.github.io/js-design-pattern/publish-subscribe-pattern/)。 
   * Redux 中分为 store, action, reducer 三部分。
   * Redux 是单项数据流， store 只能通过 reducer 获取， reducer 只能由 action 改变。
   * Redux 中的 API： subscribe, dispatch, getState<br>
   [redux简单示例](https://codesandbox.io/s/l5moll9moq)
---------------------
