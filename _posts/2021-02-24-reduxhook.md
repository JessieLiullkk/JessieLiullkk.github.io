---
title: Redux Hooks 
tags: [React, Redux Hook]
renderNumberedHeading: true
grammar_cjkRuby: true
---

const store = createStore(); params: 
createLogger
applyMiddleware

store.dispatch  params


useSelector
useDispatch
useStore

##### React-Redux
UI组件负责显示UI呈现，容器组件负责管理数据和业务逻辑
 - connect(mapStateToProps, mapDispatchtoProps)(Component)： 连接UI组件和容器组件
	 - mapStateToProps: 将外部State映射到UI组件参数的props， 返回一个对象， 会订阅Store, 当Store改变是会自动执行，重新计算UI组件参数
	 - mapDispatchtoProps: UI组件的操作映射成action（store.dispatch）函数或参数
 - Provide 组件：让容器组件拿到State, 原理是Context属性

