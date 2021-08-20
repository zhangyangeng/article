# 已看内容

常见路由任务
教程：在单页面应用中导航
教程：为英雄之旅添加路由支持（看到ActivatedRoute实战部分）

# 路由

## 编写位置

### (1).app.moudule.ts文件

在该文件中导入 `RouterModule`，然后在 `imports` 字段中填写如下代码，并在代码中书写需要定义的路由规则：
```ts
RouterModule.forRoot([]),
```

### (2).路由模块

使用如下命令创建一个默认路由配置文件
```
ng generate module app-routing --module app --flat
或
ng g m app-routing --module app --flat
```

在 `app.module.ts` 中引入该配置文件

进入该文件，引入 `RouterModule` 以及各个组件

在 `imports` 字段中填写如下代码，并在代码中书写需要定义的路由规则：
```ts
RouterModule.forRoot([]),
```

在 `exports` 字段中导出 `RouterModule`

## 路由守卫

路由守卫分为如下几种：
- `CanActive`
- `CanActiveChild`
- `CanDeactive`
- `Resolve`
- `CanLoad`

### 方法
为守卫创建服务，创建的时候可以选择守卫的类型
```
ng generate gurad 守卫名
或
ng g g 守卫名
```

在路由配置文件中按如下配置即可：
```ts
{
  path: '/your-path',
  component: YourComponent,
  canActivate: [YourGuard],
}
```

## 路由策略

&emsp;&emsp;路由器导航时其中的 URL 都存在于本地，并不会请求服务器，而当前浏览器提供两种策略来展示 URL 的样式：
- `PathLocationStrategy`：默认策略，支持 “HTML 5 pushState” 风格
- `HashLocationStrategy`：支持“hash URL”风格，即 URL 前必须加入井号才可以避免请求服务端

### 默认策略

如果使用默认策略，需要在项目的 `index.html` 中添加 `<base href>` 元素才可以正常工作

如果应用的根目录是 `app` 目录，那么可以设置如下：
```html
<head>
  <meta charset="utf-8">
  <title>RoutingApp</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
```

## 重定向路由

path：重定向源
redirectTo：重定向目标，即从空路径重定向到目标路径
pathMatch：如何匹配URL（full、prefix）