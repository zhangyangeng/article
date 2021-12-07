# 未看内容
组件：
- 视图包装
- 组件样式
- 内容投影
- 动态组件
- Angular元素

模板：
- 属性、类、样式绑定
- SVG

指令；

依赖注入：
- DI提供者

开发指南部分：

# 组件

## 组件交互

### (1).@input()

父组件通过 `@input()` 给子组件绑定属性设置输入类数据

方法如下：

- 在父组件中定义一个属性
  ```ts
  title: string = "Wrysmile's Blog";
  ```

- 在父组件的页面中给子组件传值
  ```html
  <app-title [title] = "title"></app-title>
  ```

- 在子组件中注入该属性
  ```ts
  @Input() title?: string;
  ```
  
- 在子组件的页面中即可使用该属性了
  ```html
  <p>{{title}}</p>
  ```

- 同时也可以在父组件中修改该值，会实时同步到子组件
  ```html
  // html文件
  <button (click) = "titleChange()">修改</button>
  ```
  ```ts
  // ts文件
  titleChange(){
    this.title = "Wrysmile 的小站"
  }
  ```

### (2).@output()

父组件给子组件传递一个事件，子组件通过 `@output()` 弹射触发事件

方法如下：

- 在父组件中定义属性以及触发的事件
  ```ts
  list: Array<string> = ["Angular"];
  addList(str: string){
    // 将内容添加到列表中
    this.list?.push(str);
  }
  ```
  
- 在父组件的页面中给子组件绑定该事件
  ```html
  <p *ngFor="let item of list">{{item}}</p>
  <app-title (addList) = "addList($event)"></app-title>
  ```

- 在子组件中弹射触发父组件中的事件
  ```ts
  @Output() addList = new EventEmitter();
  btnAdd(){
    this.addList.emit("Vue");
  }
  ```
  
- 在子组件的页面中触发定义好的事件
  ```html
  <button (click) = "btnAdd()">我要学Vue</button>
  ```

### (3).@ViewChild()

通过 `@ViewChild()` 来获取子组件的实例，来获取子组件的数据

该方法需要结合第二个使用，方法如下：

- 在父组件的页面中给子组件定义一个**模板引用变量**
  ```html
  <app-title #titleDom (addList) = "addList($event)"></app-title>
  <button (click) = "viewChildAdd()">父子组件交互</button>
  ```
  
- 在父组件中获取子组件的实例，然后就通过 `.child` 来调用子组件中的方法
  ```ts
  @ViewChild("titleDom") child: any;
  viewChildAdd(){
    this.child.btnAdd();
  }
  ```

- 最后在子组件中弹射触发事件即可

# 服务

## 1.创建服务

在终端中使用 `ng g s 服务名` 来创建

服务文件中需要 `@injectable()` 装饰器来进行配置，其中 `provideIn` 共有三个属性值，如下：
- `root`：注入到 `app.module.ts` 中，所有子组件都可以是用（推荐）
- `none`：不设定服务作用域（不推荐）
- `组件名`：只作用于该组件（懒加载模式）

## 2.依赖注入

需要在 `app.module.ts` 中手动进行注入

使用 `import` 引入服务，并在 `providers` 中进行注入
```ts
import { ListService } from './service/list.service';
@NgModule({
  declarations: [
    // 组件注入
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule,
    ReactiveFormsModule
  ],
  providers: [
    // 服务注入
    ListService
  ],
  bootstrap: [AppComponent]
})
```

# 路由

&emsp;&emsp;使用路由可以在 Angular 中实现路径的导航模式，具体表现就是不同视图的切换。在 Angular 中，组件的实例化与销毁、模块的加载、组件某些生命周期钩子的发起都与路由相关

**路由器** 是一个调度中心，是一套规则的列表，能够查询当前 URL 对应的规则，并呈现出相关的视图

**路由** 是列表里面的一个规则，有如下字段：
- `path`：表示该路由中的 URL 路径
- `component`：表示与该路由相关联的组件

## 1.创建路由

使用脚手架安装时会自动创建路由文件 `app-routing.module.ts`

## 2.配置路由规则

在路由文件中引入需要的组件，并在 `routes` 字段中配置规则

### (1).基础路由配置规则

即包含 URL 的路由配置，如下：
```ts
{
  path: "hello",
  component: HelloComponent
}
```

### (2).默认路由配置规则

即根据端口进入页面时默认显示，如下：
```ts
{
  path: "",
  component: HomeComponent
}
```

但默认配置状况下，如果地址栏输入错误，会显示项目的根目录网页，并不合适，使用 **通配路由配置规则** 可以解决

### (3).通配路由配置规则

即 URL 错误时显示该界面（此时可以配置404页面），如下；
```ts
{
  path: "*",
  component: HomeComponent
},
```

## 3.组件渲染输出

在根页面中使用如下语句进行渲染
```html
<router-outlet></router-outlet>
```

## 4.导航

可以在 `<a>` 标签中进行导航
```html
<a [routerLink]="['/hello']" routerLinkActive="router-link-active" >进入hello页面</a>
```

## 5.路由嵌套

在需要嵌套的路由规则中通过 `children` 字段来进行设置：
```ts
{
  path: "home",
  component: HomeComponent,
  children: [
    {
      path: "list",
      component: ListComponent
    }
  ]
}
```

在需要嵌套的页面中进行渲染
```html
<a [routerLink]="['/home/list']" routerLinkActive="router-link-active" >进入list页面</a>
<router-outlet></router-outlet>
```

## 6.路由传值

### (1).queryParams

在 `<a>` 标签中添加一个 `queryParams` 参数
```html
<a [routerLink]="['/home/list']" [queryParams]="{id: 1}" routerLinkActive="router-link-active" >进入list页面</a>
```

在子组件中注入 route 服务
```ts
constructor(
  private routerInfo: ActivatedRoute
) { }
```

然后就可以通过 `routerInfo.snapshot.queryParams` 来获取传递的参数


### (2).params

通过修改路由配置文件中的路径来进行传值
```ts
{
  path: "hello/:name",
  component: HelloComponent
},
```

修改超链接中 `routerLink` 的第二个参数
```html
<a [routerLink]="['/hello', 'params传参']"  routerLinkActive="router-link-active" >进入hello页面</a>
```

在子组件中注入 route 服务
```ts
constructor(
  private routerInfo: ActivatedRoute
) { }
```

通过 `subscribe` 订阅的方式获取 `name` 参数
```ts
this.routerInfo.params.subscribe((params: Params) => {
  this.name = params['name']
  console.log(this.name);
})
```

<strong style="color:red;">注意：使用该方式时参数位置必须对应</strong>
