# 动画


## 简单使用

### 动画定义

在组件中的 `@Component()` 装饰器的 `animations:` 属性下用代码定义你要用的动画，该属性传入的是一个数组

### 动画触发

使用 `trigger()` 函数来进行触发，其包含两个参数：
- 触发器的名字
- 数组，需要触发的动画

### 动画状态和样式

在触发器的第二个参数中定义动画状态和样式

使用 `state()` 函数来进行定义，其包含两个参数：
- 状态的名字
- 该状态的样式，使用 `style()` 函数来进行定义，其参数为一个对象，对象中传入需要设置的样式（小驼峰命名法）

### 动画转场和时序

在触发器的第二个参数中定义动画转场和时序

使用 `transition()` 函数来进行定义，其包含两个参数：
- 表达式，定义两个转场状态之间的方向
- 数组，接收一个或一系列的 `animate()` 函数，其包含3个参数
  - 持续时间，纯数字表示毫秒，表示秒需要使用字符串 `'1s'` 
  - 延迟时间，同上
  - 速度动画：`ease-in/ease-out/ease-in-out`

**执行顺序**：上面的动画转场设置会覆盖下面的动画转场设置

### 动画展示

当动画的相关配置做完以后，就可以附加到页面模板中了

给定义好的动画触发器名前面加上 @ 符号并放入方括号中，然后使用属性绑定语法来绑定到模板表达式上，如下：
```html
<div class="open-close-container" [@openClose]="isOpen ? 'open' : 'close'">
    <p>盒子现在是 {{isOpen ? '开启' : '关闭'}}！</p>
</div>
```

### 动画禁用

当我们想要禁用某元素及其子元素的动画时，可以给该元素绑定 `@.disabled`，当其值为 true 时会禁止渲染所有动画

## 转场与触发器

### 通配符

使用 `*` 通配符表示任意状态，类似于路由，我们可以把以下状态放置到最后，当所有转场都不匹配时，就使用通配符定义的转场
```ts
transition('* => *', [
    animate('1s')
]),
```

### void

**场景**：使用 void 状态可以为进入或离开页面配置转场

**使用**：可以配合通配符进行使用
- 当元素离开视图时，就会触发 `* => void(别名:leave)` 转场，而不管它离开前处于什么状态
- 当元素进入视图时，就会触发 `void => *(别名:enter)` 转场，而不管它进入时处于什么状态

**注意**：
- 通配符状态 `*` 会匹配任何状态 —— 包括 `void`
- 使用别名时可以与 `ngIf/ngFor` 一起使用

### 父子动画

每次在 Angular 中触发动画时，父动画始终会优先，而子动画会被阻塞

为了运行子动画，父动画必须查询出包含子动画的每个元素，然后使用 `animateChild()` 函数来运行它们

若父动画被禁用时，子动画可以使用如下方式运行：
- 父动画可以使用 `query()` 函数来收集 HTML 模板中位于禁止动画区域内部的元素。这些元素仍然可以播放动画
- 子动画可以被父动画查询，并且之后使用 `animateChild()` 来播放它

## 复杂序列

用于控制复杂序列的函数有：
- `query()`：查找一个或多个内部 HTML 元素，其包含两个参数：
  - css 选择器
  - 数组，传入需要设置的样式和 `stagger()` 函数
- `stagger()`：为多元素动画应用级联延迟，其包含两个参数：
  - 延迟时间
  - 数组，传入一个或多个 `animate()` 
- `group()`：并行执行多个动画步骤，其包含一个数组参数，传入一个或多个 `animate()`
- `sequence()`：逐个顺序执行多个动画步骤，其包含两个参数：
  - `style()` 函数用来立即应用所指定的样式数据
  - `animate()` 函数用来在一定的时间间隔内应用样式数据

## 可复用动画

可以将动画单独定义在一个文件中，然后在需要的组件中引入即可

使用方法如下：
- 在 ts 文件中导出一个用 const 定义的变量，其包含两个参数：`style()` 和 `animate()`
```ts
export const transAnimation = animation([
    style({
        height: '{{height}}',
        opacity: '{{opacity}}',
        backgroundColor: '{{backgroundColor}}'
    }),
    animate('{{time}}')
]);
```

- 在组件设置的 `animation()` 函数的数组中使用 `useAnimation()` 函数来调用自定义动画，其包含两个参数：动画文件名和参数配置对象
```ts
transition('open => close', [
  useAnimation(transAnimation, {
    params: {
      height: 0,
      opacity: 1,
      backgroundColor: 'red',
      time: '1s'
    }
  })
]),
```

# 错误

## ng build 后的错误

参考[这里](https://blog.csdn.net/muguli2008/article/details/105653433/)


# 补充内容
# 路由

如果创建项目时没有使用默认配置创建路由器，可以使用如下命令进行创建，其中参数如下：
- `--flat` 把这个文件放进了 `src/app` 中，而不是单独的目录中
- `--module=app` 告诉 CLI 把它注册到 `AppModule` 的 `imports` 数组中
```
ng generate module app-routing --flat --module=app
```

然后用下面的代码替换生成的文件中的代码：
```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```