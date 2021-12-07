# 已看内容
视图包装
组件样式
指令
依赖注入

# 组件

## 视图包装

在 Angular 中，组件的 CSS 样式被封装进了自己的视图，可以通过在组件的元数据上设置**视图封装模式**，你可以分别控制**每个组件**的封装模式

封装模式如下：
- `ShadowDom`：使用浏览器原生的 Shadow DOM 来实现（不进不出，全局样式进不来，组件样式出不去）
- `Emulated`：默认值，通过预处理 CSS 代码来模拟 Shadow DOM 的行为（只进不出，全局样式能进来，组件样式出不去）
- `None`：不使用视图封装，即把 CSS 添加到全局样式中（能进能出）

设置封装模式：组件元数据中的 `encapsulation` 属性来设置组件封装模式
```ts
encapsulation: ViewEncapsulation.ShadowDom
```

## 组件样式

### :host

作用：用来选择当前组件中的子元素

场景：若想给某组件中的所有元素设置 flex 布局，可以不需要单独加一个盒子，而直接通过该伪类选择器给该组件设置 flex 布局

引申：
- `:host(.active)`符合该类的组件
- `:host h2`组件中符合该选择器的元素

### :host-content

作用：用来选择当前组件中的祖先节点


# 指令

## 1.内置指令

内置属性型指令：

- NgClass：添加和删除一组 CSS 类

- NgStyle：添加和删除一组 HTML 样式

- NgModel：将数据双向绑定添加到 HTML 表单元素

内置结构型指令：

- NgIf：从模板中创建和销毁子视图

- NgFor：为列表中的每个条目重复渲染一个节点

- NgSwitch：一组在备用视图之间切换的指令

## 2.属性型指令

建立属性型指令：
```cmd
ng generate directive xxx
或
ng g d xxx
```

应用属性型指令：只要在相应的 HTML 模板中应用指令选择器名即可

## 3.结构型指令

简写形式： Angular 将结构型指令前面的星号转换为围绕宿主元素及其后代的 `<ng-template>`

# 服务

## 创建服务

使用
```
ng generate service 服务名
或
ng g s 服务名
```

最好在单独的文件中定义组件和服务，如果需要合并在同一个文件中，则必须<strong style="color:red;">先定义服务，再定义组件</strong>（顺序不对时会返回运行时的空引用错误）

## 依赖注入

依赖项注入（DI）是一种设计模式，在这种设计模式中，类会从外部源请求依赖项而不是创建它们

在组件中注入服务时需要将依赖项注入组件的 `constructor()` 中

## 服务嵌套

当某个服务(B)依赖于另一个服务(A)时，需要在该服务(B)中注入另一个服务(A)
```ts
import { Injectable } from '@angular/core';
import { HEROES } from './mock-heroes';
import { Logger } from '../logger.service';
@Injectable({
  providedIn: 'root',
})
export class HeroService {
  // 注入Logger服务
  constructor(private logger: Logger) {  }
  getHeroes() {
    this.logger.log('Getting heroes ...');
    return HEROES;
  }
}
```