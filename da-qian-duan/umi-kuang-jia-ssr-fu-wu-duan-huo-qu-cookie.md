# umi框架SSR 服务端获取cookie

#### 在项目核心文件server.js 中增加传入自定义参数到ctx中

```javascript
app.use(async (ctx, next) => { 
    const { html, error } = await render({ 
        path: ctx.request.url, 
        getInitialPropsCtx: { 
            cookie: ctx.request.header.cookie, 
        }
    }); 
    ctx.body = html; 
}
```

#### 然后在业务js的getInitialProps方法中进行获取打印

```javascript
Home.getInitialProps = (async (ctx: any) => { 
  console.debug('**********************', ctx.cookie) 
})
```

> <mark style="color:red;">需要注意的是，参数只有在服务端渲染时才可以生效，本地的dev调试模式是无法获取的。只有实际build完成后用pm2运行，才可以看到实际效果</mark>
