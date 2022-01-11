egg-logger 通用错误，自定义的，类似于知道错误了，需要自己打日志，举例子：
fetch api -> { status: 1000 } // logger.warn(......)   return res { ok, data: '' }

通过

```js
//这种都是error        
ctx.runInBackground(async () => {
            // 这里面的异常都会统统被 Backgroud 捕获掉，并打印错误日志
            await ctx.service.sAliLog.check(ctx.query);
        });

    setImmediate(() => {
      ctx.service.trade.check(request).catch(err => ctx.logger.warn(err));
    });
```

可以在前段代码不抛出异常，即逻辑通顺时只记录日志



业务性错误，运行时错误或者业务逻辑调用错误
  const r1 = await api1   
 const r2 = await api2
......
api2 

把对应代码块try..catch起来，throw出对应错误，在中间件统一处理并上报打点

类似：

```js
module.exports = () => {
    return async function errorHandler(ctx, next) {
        try {
            await next();
        } catch (err) {
            const { app, helper } = ctx;
            app.emit('error', err, ctx);
            ctx.body = helper.resTemplate(err.message, "对应错误信息");
            app.logger.error("对应错误信息")
            ctx.status = 333;
        }
    }
}
```



egg-onerror  status 404 403 301 302...

在配置文件预配置，或者在中间件中预配置