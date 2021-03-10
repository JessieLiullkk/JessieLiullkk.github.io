---
title: React-Redux Hooks 
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


##### Context - 创建上下文
```bash
const ColorContext = React.createText("green"); // defaultValue: green
```
###### 顶层组件
```bash
外层有Provide
<ColorContext.Provide value="red">
	<ContextComponent />  // red
</ColorContext.Provide>

无Provide
<ContextComponent />  // green

只有当组件所处的树中没有匹配到 Provider 时，其 defaultValue 参数才会生效。
```

###### 底层组件如何订阅

 - React component:

```
ReactComponentName.contextType = ColorContext;
or
static contextType = ColorContext;

component中调用： this.context
```

 - Function component:

```
<ColorContext.Consumer>
	{value => (...)}
</<ColorContext.Consumer>>
```

redux中间件的理解


