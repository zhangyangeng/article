# 已看内容

组件：
- 父子组件共享数据
- 内容投影
- 动态组件

模板：
- 用 svg 做模板

# 组件
@Output() 必须是 `EventEmitter` 类型的

## 内容投影

### (1).单插槽内容投影

使用 `<ng-content>` 来进行投影

### (2).多插槽内容投影

使用 `<ng-content select="[question]">` 来进行投影

## 动态组件


# 路由

## 创建路由

使用 `ng new routing-app --routing` 来生成一个带有应用路由模块的基本项目

## 路由顺序

**Router**在匹配路由时遵循的是**先到先得**的策略，所以路由顺序如下：
- 先配置静态路径的路由
- 再配置与默认路由匹配的空路径路由
- 最后配置通配符路径

### 通配

当所请求的 URL 与任何路由器路径都不匹配时，就会选择该路由

作用：展示404页面或跳转到应用的主组件

**两个星号

### 重定向路由

当时用重定向路由时，浏览器会默认跳转到该路由上

重定向路由需要设置以下三个参数：
- `path`：重定向源
- `redirectTo`：重定向目标
- `pathMatch`：如何匹配URL（full、prefix）

配置如下：
```ts
{
    path: 'home',
    component: HomeComponent
},
{
    path: '',
    redirectTo: '/home',
    pathMatch: 'full'
},
```