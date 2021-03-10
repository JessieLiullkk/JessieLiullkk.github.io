---
title: React-Redux Hooks 
tags: [React, Redux Hook]
renderNumberedHeading: true
grammar_cjkRuby: true
---

##### React-Redux
UI组件负责显示UI呈现，容器组件负责管理数据和业务逻辑
 - connect(mapStateToProps, mapDispatchtoProps)(Component)： 连接UI组件和容器组件
	 - mapStateToProps: 将外部State映射到UI组件参数的props， 返回一个对象， 会订阅Store, 当Store改变是会自动执行，重新计算UI组件参数
	 - mapDispatchtoProps: UI组件的操作映射成action（store.dispatch）函数或参数
 - Provide 组件：让容器组件拿到State, 原理是Context属性

##### React-Redux Hook: connect()高阶组件的替代品

 - useSelector: 获取store中的state
	 - 与mapStateToProps的区别: 
		 - 返回值可以是任意类型，不一定非要是对象
		 - 当 分发(dispatch) 了一个 action 时，useSelector() 会将上一次调用 selector 函数结果与当前调用的结果进行引用(===)比较，如果不一样，组件会被强制重新渲染。如果一样，就不会被重新渲染。
		 - 默认为===严格比较， connect浅比较
 - useDispatch
   ```
   const mapDispatchToProps=.....
   const dispatch = useDispatch();
   const actions = mapDispatchToProps(dispatch);
   ```
   
 - useStore 获取整个store

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


