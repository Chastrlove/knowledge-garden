---
date created: 2022-09-02
date modified: 2022-09-02
title: 从react组件中提取props
---

```typescript
declare type $ElementProps<T> = T extends React.ComponentType<infer Props>
? Props extends object
  ? Props
  : never
: never;
```
