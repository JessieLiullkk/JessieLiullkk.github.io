---
title: React Hook
tags: [React-Hook, useMemo, useCallback]
renderNumberedHeading: true
grammar_cjkRuby: true
---

useMemo useCallback的目的：
理清页面的重新渲染依赖于什么值的改变，优化不需要的计算。

#### useMemo
deps改变时才会重新计算
```bash
const calculate = useMemo(() => {
	// CREATE
	// return a value
},[deps])
```

#### useCallback
使用场景是：有一个父组件，其中包含子组件，子组件接收一个函数作为props；通常而言，如果父组件更新了，子组件也会执行更新；但是大多数场景下，更新是没有必要的，我们可以借助useCallback来返回函数，然后把这个函数作为props传递给子组件；这样，子组件就能避免不必要的更新

meas: 用useCallback包裹的函数只有在依赖项更新时才更新。

#### useEffect 
```markdown
A) Not provided: the side-effect runs after every rendering.
B) An empty array [ ]: the side-effect runs once after the initial rendering.
C) Has props or state values [prop1, prop2, ..., state1, state2]: the side-effect runs only when any depenendecy value changes.
```
### deps is [ ]
```markdown
componentWillUnmount will trigger the callback
```
### deps will change 

![useEffect callback](https://raw.githubusercontent.com/JessieLau-CT/images/main/小书匠/1640068355887.png)

```markdown
import React, { useEffect, useState } from "react";
import "./styles.css";

function RepeatMessage({ message }) {
  useEffect(() => {
    const id = setInterval(() => {
      console.log(message);
    }, 2000);
    return () => {
      clearInterval(id);
    };
  }, [message]);

  return <div className="message">I'm logging to console "{message}"</div>;
}

export default function App() {
  const [message, setMessage] = useState("Hello, World!");

  return (
    <div className="App">
      <h3>Type the message to log to console</h3>
      <input
        type="text"
        value={message}
        onChange={(e) => setMessage(e.target.value)}
      />
      <RepeatMessage message={message} />
    </div>
  );
```

refer to: https://dmitripavlutin.com/react-useeffect-explanation/