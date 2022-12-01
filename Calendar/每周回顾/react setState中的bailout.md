---
date created: 2022-09-20
date modified: 2022-09-20
title: react setState中的bailout
---

```js
const Parent = () => {

  const [state, setState] = useState(0);

  console.log('Log parent re-renders');

  return (

    <>

      <button onClick={() => setState(1)}>Click me</button>

    </>

  )

}
```

结果：你会看到，第一次点击按钮console.log被触发：这是预期的，我们把状态从0变成了1。但是第二次点击，我们把状态从1变成了1，这应该是保释，也会触发console.log!但第三次和接下来的所有点击都没有任何作用。  
[useState not bailing out when state does not change · Issue #14994 · facebook/react · GitHub](https://github.com/facebook/react/issues/14994#issuecomment-468779750)
