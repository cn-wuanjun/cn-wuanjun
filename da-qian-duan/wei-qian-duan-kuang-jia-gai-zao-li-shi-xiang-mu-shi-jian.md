---
description: Next.js+Layui+qiankun
---

# 微前端框架改造历史项目实践

## 改造主项目

```
yarn add qiankun
```

在业务代码中引入子应用

```javascript
async componentDidMount() {
    const { registerMicroApps, start ,loadMicroApp} =  require('qiankun')

    const checkExist = setInterval(() => {
      if (document.querySelector('#personCenter')) {
        clearInterval(checkExist);
        loadMicroApp({
          name: 'personCenter',
          entry: '//localhost:8080/',
          container: '#personCenter',
        }, {
          excludeAssetFilter: (url) => {
            return url.indexOf('.js') !== -1
          },
        });
      }
    }, 1000);
    
}
```

需要注意，在加载子应用时，需要确保挂载点div已经渲染出来了。

## 改造子应用

按照官方步骤配置子应用：[https://qiankun.umijs.org/zh/guide/tutorial#%E9%9D%9E-webpack-%E6%9E%84%E5%BB%BA%E7%9A%84%E5%BE%AE%E5%BA%94%E7%94%A8](https://qiankun.umijs.org/zh/guide/tutorial#%E9%9D%9E-webpack-%E6%9E%84%E5%BB%BA%E7%9A%84%E5%BE%AE%E5%BA%94%E7%94%A8)



每个页面中都确保加载`entry.js`

## 注意点

1. 子应用是一个layui的jQuery项目，需要解决js资源引用的问题，动态加载的js资源会被qiankun屏蔽掉，如果要加载需要配置excludeAssetFilter参数。([https://qiankun.umijs.org/zh/api#loadmicroappapp-configuration](https://qiankun.umijs.org/zh/api#loadmicroappapp-configuration))
2. layui作为qiankun的子应用时，出现错误：ReferenceError: layer is not defined
   1. 配置{ sandbox: false } 禁用沙箱，排查问题
   2. 确认Layui在子应用独立运行时是否工作正常。如果独立运行正常，则说明问题可能在于子应用作为Qiankun子应用时的环境配置。

## Todo

* [ ] layui子应用中的js引用从动态修改为静态\


