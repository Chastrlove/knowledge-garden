---
date created: 2022-09-13
date modified: 2022-09-13
title: 动态修改Webpack.DefinePlugin如何生效
---

![](https://raw.githubusercontent.com/Chastrlove/picgo/main/20220913104833.png)

![](https://raw.githubusercontent.com/Chastrlove/picgo/main/20220913105710.png)

![](https://raw.githubusercontent.com/Chastrlove/picgo/main/20220913104919.png

![](https://raw.githubusercontent.com/Chastrlove/picgo/main/20220913105634.png)

```js
webpackConfig.plugins.forEach((plugin) => {  
  // we look for the DefinePlugin definitions so we can  
  // update them on the active compilers  if (plugin && typeof plugin.definitions === "object") {  
    Object.keys(plugin.definitions).forEach((key) => {  
      if (key === "Platform") {  
        plugin.definitions[key] = Math.random(); 
      }  
    });  
  });  
server.invalidate();
```

在newCompilation的过程中触发了DefinePlugin中注册的compiler.hooks.compilation钩子，获取了最新的definitions，同时设置了compilation.valueCacheVersions。  
在needBuild阶段，发现valueCacheVersions中的definitions值与valueDependencies中的值不相同，于是触发了module.build。在build过程中，会valueDependencies赋予新的值

```js
needBuild(context, callback)
{
    const {fileSystemInfo, compilation, valueCacheVersions} = context;
    // build if enforced  
    if (this._forceBuild) return callback(null, true);

    // always try to build in case of an error  
    if (this.error) return callback(null, true);

    // always build when module is not cacheable  
    if (!this.buildInfo.cacheable) return callback(null, true);

    // build when there is no snapshot to check  
    if (!this.buildInfo.snapshot) return callback(null, true);

    // build when valueDependencies have changed  
    /** @type {Map<string, string | Set<string>>} */
    const valueDependencies = this.buildInfo.valueDependencies;
    if (valueDependencies) {
        if (!valueCacheVersions) return callback(null, true);
        for (const [key, value] of valueDependencies) {
            if (value === undefined) return callback(null, true);
            const current = valueCacheVersions.get(key);
            if (
                value !== current &&
                (typeof value === "string" ||
                    typeof current === "string" ||
                    current === undefined ||
                    !isSubset(value, current))
            ) {
                return callback(null, true);
            }
        }
    }
    //省略
}
```

References:  
[webpack5 打包流程源码剖析（1） - Jacob的技术博客](https://homecpp.art/5506/361/1858)  
[webpack5 配置14 打包分析+webpack源码_coderlin_的博客-CSDN博客_webpack5源码分析](https://blog.csdn.net/lin_fightin/article/details/115643185)
