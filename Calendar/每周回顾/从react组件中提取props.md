---
date created: 2022-09-02
date modified: 2022-09-13
title: 从react组件中提取props
---

```ts
declare type $ElementProps<T> = T extends React.ComponentType<infer Props>
? Props extends object
  ? Props
  : never
: never;
```
