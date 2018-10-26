# 基于golang语言web框架gin搭建应用-04性能监控

使用gin自带的中间件

```
govendor fetch github.com/gin-contrib/pprof
```

在路由的addRouter函数中增加下面的代码

```
// TODO: only admin user can access pprof router
pprof.Register(router, "dev/pprof")
```

启动应用，浏览器打开[http://localhost:8000/dev/pprof/](http://localhost:8000/dev/pprof/)

