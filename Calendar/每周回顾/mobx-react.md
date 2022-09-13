---
date created: 2022-09-01
date modified: 2022-09-02
title: mobx-react-lite如何连接react与mobx
---

# mobx-react-lite如何连接react与mobx

```javascript

observer(ReactComponent)

useObserver(()=>ReactComponent(props,ref))


useObserver利用Mobx中的Reaction

const newReaction = new Reaction('组件观察者', () => {
	forceUpdate()
})

newReaction.track(()=>{
   rendering = fn()
})


```
