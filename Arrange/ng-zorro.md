# 基本知识

## 1.自动安装

新建一个 Angular 项目：
```
ng new project
```

进入项目目录，完成组件的初始化，包括引入国际化文件、导入模块、引入样式文件等：
```
ng add ng-zorro-antd
```

运行项目
```
ng serve --open
```

## 2.手动安装

>如果项目本身已经存在时可以手动进行安装该组件

安装组件：
```
npm install ng-zorro-antd --save
```

<strong style="color:red;">使用全局样式：</strong>

- 在 `angular.json` 中引入全部组件样式:
```json
{
  "styles": [
    "node_modules/ng-zorro-antd/ng-zorro-antd.min.css"
  ]
}
```

- 在 `style.css` 中引入预构建样式文件:
```css
@import "~ng-zorro-antd/ng-zorro-antd.min.css";
```

<strong style="color:red;">使用特定组件样式：</strong>

- 先引入基本样式，再引入组件样式
```css
@import "~ng-zorro-antd/style/index.min.css"; /* 引入基本样式 */
@import "~ng-zorro-antd/button/style/index.min.css"; /* 引入组件样式 */
```

在 `app.module.ts` 中引入组件模块：
```ts
import { NgModule } from '@angular/core';
import { NzButtonModule } from 'ng-zorro-antd/button';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    NzButtonModule
  ]
})
export class AppModule { }
```

在模板中使用：
```html
<button nz-button nzType="primary">Primary</button>
```

# 组件相关

## 通用方法

找到各个组件的模块引入语句：
- 如果整个项目只有 app 一个 module，那么就需要在它中引入，并加入到 imports 数组中
- 如果整个项目中还有其他 module，且是该 module 下的页面需要引用该组件，那么加需要在 该 module 中引入，并加入到 imports 数组中

复制组件代码，粘贴到需要使用的页面中即可

## 图标

### (1).静态加载

官方推荐<strong style="color:red;">只加载需要的图标</strong>，方法如下：
- 先引入图标模块
- 然后找到需要的图标，引入该图标，命名方式如下：
  - 先首字母大写，如果有 `-`，去掉连字符并将连字符后面的单词首字母大写，最后接着图标主题风格(Outline、Fill、TwoTone)
  - 例：`account-book` 即为 `AccountBookOutline` 
```ts
import { AccountBookOutline, AccountBookFill, AccountBookTwoTone } from '@ant-design/icons-angular/icons';
```

- 定义一个数组来接收这些图标
```ts
import { IconDefinition } from '@ant-design/icons-angular';
const icons: IconDefinition[] = [AccountBookOutline, AccountBookFill, AccountBookTwoTone];
```

- 修改 imports 数组
```ts
NzIconModule.forRoot(icons)
```

- 最后将组件代码复制进去即可

----

官方不推荐的<strong style="color:red;">加载全部图标</strong>，请前往 [这里](https://ng.ant.design/components/icon/zh) 查看

### (2).动态加载

引入图标模块

修改 `angular.json` 文件如下：
```json
"assets": [
  {
    "glob": "**/*",
    "input": "./node_modules/@ant-design/icons-angular/src/inline-svg/",
    "output": "/assets/"
  }
],
```

重新编译项目即可正常加载图标


# 项目实战

[待办事项](https://zhuanlan.zhihu.com/p/38373638)

https://juejin.cn/post/6844903778164949000#heading-36