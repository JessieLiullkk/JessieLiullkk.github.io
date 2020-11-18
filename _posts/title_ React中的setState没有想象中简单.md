---
title: React中的setState没有想象中简单
---
最近看到一些文章关于setState的用法，在这里做一个笔记。

## setState不为人知的参数

### 第一个参数：不仅能接受一个对象，还能接受一个函数

#### 该函数接受该组件前一刻的 state 以及当前的 props 作为参数，计算和返回下一刻的 state：

``` bash
this.setState(function (state, props) {
 return {
  score: state.score - 1
 }
});
```

### 第二个参数：返回当前计算后的state状态

``` bash
handle() {
    this.setState((prevState) => ({ count: prevState.count + 1 }), () => {
        console.log(this.state)
    })
}
```

## setState是同步还是异步？
### 作为一个react初学者，看过一些文章之后我才发现setState是有异步和同步的区分的，所以如果没有很深入的去研究一个框架，很多东西你学会的也是表面的。

## 生命周期中的setState
``` bash
class App extends Component {
  state={
    count:0
  }
  componentDidMount(){
    this.setState((prevState) =>({
      count:prevState.count+1
   }),()=>{console.log("setState",this.state)} // 后输出 setState {count: 1}
   )
   console.log(this.state) // 先输出 {count: 0}
  }
  render() {
    return (
      <div className="App">
        <button>{this.state.count}</button>
      </div>
    );
  }
}
```

### React引发的事件，如onClick/onChange

``` bash
 state={
    count:0
  }
  handle = () => {
    this.setState((prevState) =>({
       count:prevState.count+1
    }),()=>{console.log("setState",this.state)} // 输出修改后的值
    )
    console.log(this.state) // 输出上一个state的
  }
  render() {
    return (
      <div className="App">
        <button onClick={this.handle}>{this.state.count}</button>
      </div>
    );
  }
```
### 原生事件的setState:通过addEventListener添加的事件
```bash
state={
    count:0
  }
  handle = () => {
    this.setState((prevState) =>({
       count:prevState.count+1
    }),()=>{console.log("setState",this.state)} // 两次输出一致，都是修改后的state值
    )
    console.log(this.state) // 两次输出一致，都是修改后的state值
  }
  componentDidMount(){
    document.getElementById('myBtn').addEventListener('click',this.handle)
  }
  render() {
    return (
      <div className="App">
        <button id='myBtn'>{this.state.count}</button>
      </div>
    );
  }
```

### setTimeout/setInterval中的setState
```bash
 state={
    count:0
  }
  componentDidMount(){
    setTimeout(()=>{
      this.setState({
        count:this.state.count+1
      })
      console.log("1",this.state)// 修改后的state值 {count: 1}
    },1000)
    console.log("2",this.state) // {count: 0}
  }
  render() {
    return (
      <div className="App">
        <button id='myBtn'>{this.state.count}</button>
      </div>
    );
  }
```
## setState的批量合并
看到一些文章说setState遇到批量更新的会合并，，其实，是分以下两种写法的
```bash
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
  // 这种情况是会合并且只执行最后一次
```
```bash
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  // 这种情况是输出的值是三次累加的结果
```

例子：在React引起的事件
```bash
 state={
    count:0
  }
  handle = () => {
    this.setState({
      count:this.state.count+1
    })
    this.setState({
      count:this.state.count+1
    })
     // 每次只加1
  }
  render() {
    return (
      <div className="App">
        <button onClick={this.handle}>{this.state.count}</button>
      </div>
    );
  }
```
```bash
 state={
    count:0
  }
  handle = () => {
    this.setState((prevState) =>({
       count:prevState.count+1
    }))
    this.setState((prevState) =>({
      count:prevState.count+1
    }))
   // 每次加2
  }
  render() {
    return (
      <div className="App">
        <button onClick={this.handle}>{this.state.count}</button>
      </div>
    );
  }
```
在生命周期中
```bash
componentDidMount () {
   this.setState({
     count:this.state.count+1
   })
   this.setState({
    count:this.state.count+1
  })
 }
```
```bash
  componentDidMount () {
    this.setState((prevState) =>({
       count:prevState.count+1
    }))
    this.setState((prevState) =>({
      count:prevState.count+1
    }))
  }
```
在setTimeout中
```bash
componentDidMount () {
   setTimeout(()=>{
     this.setState({
       count:this.state.count+1
     })
     this.setState({
      count:this.state.count+1
    })
    this.setState({
      count:this.state.count+1
    })
   },1000)
  }
  render() {
    return (
      <div className="App">
        <button >{this.state.count}</button> //3
      </div>
    );
  }

```

总结：
1.setState为异步：在react的生命周期及由react引起的事件（onClick/onChange）中
2.setState为同步：由原生事件引起的及在setTimeout/setInterval中
3.setState批量操作是否会合并：当setState为异步时且参数为对象，取最后一次来执行，若参数为函数，则每一次都会计算；当setState为同步时，不管参数是对象还是函数，都会执行。