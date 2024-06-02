
### 1.封装一个自带了ui组件的类自定义hooks
用react官网的例子改造一下，比较像自定义hooks，但是不完全是。闭包中提供了一个ui组件以及这个ui组件的修改方法，以此来进行外部的额外干预
```
import { useState } from 'react';

export default function MyApp() {
  const {MyButtonUI:MyButtonUI1,handleClick:handleClick1}  = MyButton()
  const {MyButtonUI:MyButtonUI2,handleClick:handleClick2}  = MyButton()
  return (
    <div>
      <h1>Counters that update separately</h1>
      <button onClick={()=>{
          handleClick1()
          handleClick2()
      }}>all</button>
      <MyButtonUI1 />
      <MyButtonUI2 />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }
  const MyButtonUI=()=>{
    return <button onClick={handleClick}>
      Clicked {count} times
    </button>
  }
  return {
    MyButtonUI,
    handleClick
  };
}

```
