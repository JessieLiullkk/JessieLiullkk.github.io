---
title: 关于React你必须知道的事
---

** 一、React component的两种写法**
1. 函数式定义的无状态组件
特点：没有state/生命周期
```bash
export default function App(props) {
  return <input />
}
```
2.ES6方式定义的组件
```bash
class List extends React.Component {
    render() {
        return <div>我是一个es6方式定义的react组件</div>
    }
}
```
** 使用场景：**
----无状态组件适用于纯展示或props传递渲染时

** 二、React.Component 绑定方法的几种方法**
1.在construct中绑定
```bash
class List extends React.Component {
    constructor(props) {
        super(props)
        this.onClickList = this.onClickList.bind(this)
    }
    onClickList() {
        console.log('我被点了')
    }
    render() {
        return <div onClick={this.onClickList}>点我点我点我</div>
    }
}
```
2.在render中bind
```bash
class List extends React.Component {
    onClickList() {
        console.log('我被点了')
    }
    render() {
        return <div onClick={this.onClickList.bind(this)}>点我点我点我</div>
    }
}
```
3.方法使用箭头函数，不需要bind
```bash
class List extends React.Component {
    onClickList = () => {
        console.log('我被点了')
    }
    render() {
        return <div onClick={this.onClickList}>点我点我点我</div>
    }
}
```
4.render内用箭头函数
```bash
class List extends React.Component {
    onClickList() {
        console.log('我被点了')
    }
    render() {
        return <div onClick={() => this.onClickList()}>点我点我点我</div>
    }
}
```
** 对于在方法上传参**
```bash
<button onClick={this.handleClick.bind(this, id)} />
<button onClick={() => this.handleClick(id)} />
```
** 三、为什么循环渲染组件的要加上key**
key相同则后面的会被丢弃，不会渲染---- react根据key来决定是销毁重新创建组件还是更新组件
	* key相同，若组件属性有所变化，则react只更新组件对应的属性；没有变化则不更新。
	* key值不同，则react先销毁该组件(有状态组件的componentWillUnmount会执行)，然后重新创建该组件（有状态组件的constructor和componentWillUnmount都会执行）
	*不推荐使用map方法的index作为key，因为这个key不是唯一标识当前组件,最好使用后台返回的id
```bash
class App extends React.Component{
  state={
    data:[1,2,3,4]
  }
  componentDidMount(){
    //全部re-render
    setTimeout(()=>{
      this.setState({
        data:[4,1,2,3]     
    })},10000)
    
    //只会re-render最后一个div
    setTimeout(()=>{
      this.setState({
        data:[1,2,3,5]     
    })},10000)
  }
  render(){
    return(
      <div>{      
        this.state.data.map((item,index)=>(
          <div key={index}>{item}</div>
        ))
      }
      </div>
      
    )
  }
}
export default App;
```
** 四、React的ref揭秘**
** ref的三种用法:** 字符串(已废弃) ；回调函； React.createRef() （React16.3提供）
1.字符串:要将获取dom节点写在未render之前的生命周期内，在render之前dom节点未渲染，会报错
```bash
handle=()=>{
    console.log(this.refs.textInput.value)
  }
  render() {
      return (
          <input ref="textInput" onChange={this.handle}/>
      )
  }
```
2.回调函数：函数的参数是dom节点或组件实例，不能再render之前调用
```bash
class App extends Component {
  componentDidMount() {
      console.log(this.textInput.value)
  }
  handle=()=>{
    console.log(this.textInput.value)
  }
  render() {
      return (
          <input ref={(inputs)=>{this.textInput=inputs}} onChange={this.handle} />
      )
  }
}
export default App;
```
3.React.createRef():this.myRef.current可以拿到input实例
```bash
class App extends React.Component{
  constructor(props){
      super(props);
      this.myRef=React.createRef();
  }
  componentDidMount(){
      console.log(this.myRef.current.value); // 2
  }
  render(){
      return <input ref={this.myRef} value='2'/>
  }
}
export default App;
```

### **注意：**
ref在函数式组件上不可使用，函数式组件无实例

**五、props/state/this.的区别**
props:父子组件之间的传递，且在子组件内不可更改
state:当前组件内的状态，
this.:当时面试官问我这个的时候我有点发懵，还有this.的用法？？回来一想，我觉得自己被props和state固话了一种模式，觉得组件中的数据只能存放在这两种模式下
this.可以放定时器的id,组件内调用函数也是用this.方法(),this.setState({})
参考：https://www.jianshu.com/p/4d2838ae7b29


