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





# 项目实战

[待办事项](https://zhuanlan.zhihu.com/p/38373638)

https://juejin.cn/post/6844903778164949000#heading-36