# 国际化

## 步骤

### (1).添加 localize 包

在项目根目录中使用以下命令添加 localize 包到项目中
```
ng add @angular/localize
```


### (2).标记要翻译的文本

Html 文件只需要在需要进行翻译的标签中添加 `i18n`，如下：
```html
<h2 i18n>Top Heroes</h2>
```

TypeScript 文件只需要在构造函数中指定以下语句即可：
```ts
title = 'Tour of Heroes';
constructor(){
  this.title = $localize `Tour of Heroes`;
}
```

### (3).提取源语言文件

使用下面的命令将标记好的文本提取到源语言文件中
```
ng extract-i18n								// 在项目的根目录中生成源语言文件
ng extract-i18n --output-path src/locale	// 修改生成地址到 src/locale
```

参数说明：
- `--output-path`：改变生成文件的位置
- `--format`：改变生成文件的格式（XLIFF1.2、XLIFF2、XML消息包）
- `--outFile`：改变生成文件的文件名

### (4).生成语言文件副本

提取出来的源语言文件是一个叫做 `messages.xlf`，当有不同语言需求时需要为不同语言生成源语言文件副本

复制并重命名为 `messages.zh.xlf`，zh 即为目标语言

### (5).翻译语言文件副本

交由专业的翻译人员使用 XLIFF 文件编辑器来创建和编辑翻译

打开该文件，将 `<source></source>` 标签复制并粘贴在其下方，改为 `<target></target>` 标签，然后翻译标签中的文本即可
```html
<source>Dashboard</source>
<target>仪表盘</target>
```

### (6).定义本地环境

修改项目的 `Angular.json` 中相关配置，修改如下：
```json
"你的项目名": {
  "i18n": {
    "sourceLocale": "en-US",              // 当前本地环境语言，en表示语言，US表示国家，这里即美国英语区
    "locales": {						  // 本地环境标识符到翻译文件的映射表
      "zh": "src/locale/messages.zh.xlf"
    }
  },
  "architect": {
    "build": {
      "options": {
        "localize": true,				  // true表示为每种本地环境生成一个应用版本
        "aot": true,					  // 本地化组件模块需要进行预先编译
      }
      "configurations": {
        "zh": {							  // 使用ng serve时配置
          "localize": [
            "zh"
          ]
        }
      }
    }
    "serve": {
      "configurations": {
        "zh": {							  // 使用ng serve 时配置
          "browserTarget": "你的项目名:build:zh"
        }
      }
    }
  }
}
```

### (7).合并翻译文件

构建项目时使用以下命令：
```
ng build --localize
```

直接在本地运行项目时需要将上一点中后面两个进行配置后使用以下命令：
```
ng serve --open --configuration=zh
```

### 引申：生成翻译文件时自动追加新内容

> 直接步骤操作时，当增加新内容时都会重新生成翻译文件，之前翻译过的内容还需重新翻译并不方便，所以可以使用插件来进行追加翻译，参考自 [这里](https://www.cnblogs.com/chen8840/p/14338895.html)

导入 `ngx-i18nsupport` 包：
```
npm install ngx-i18nsupport --save-dev
```

在项目的根目录中新增 `xliffmerge.json` 文件，并写入如下内容：
```json
{
  "xliffmergeOptions": {
    "srcDir": "src/locale",
    "genDir": "src/locale"
  }
}
```

在 `package.json` 文件中添加翻译合并脚本：
```json
"scripts": {
  // 生成翻译文件并存放在指定目录下
  "extract-i18n": "ng extract-i18n --output-path src/locale",
  // 调用配置文件将修改追加到指定语言的翻译文件中
  "xliffmerge": "xliffmerge --profile xliffmerge.json zh"
},
```

之后在生成翻译文件时执行下面语句即可生成且追加修改了：
```
npm run extract-i18n;npm run xliffmerge;
```

最后合并翻译文件即可


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

# 创建项目中

ng new project -S：表示不使用测试

<hr/>

>9.6补充内容

# 服务端渲染

## 基础

使用 **Express** 来进行渲染

## 优点

通过搜索引擎优化(SEO)来帮助网络爬虫
提升在手机和低功耗设备上的性能
迅速显示出第一个支持首次内容绘制(FCP)的页面

## 使用方法

在项目根目录的命令行窗口使用如下命令创建 `app.server.module.ts`：
```
ng add @nguniversal/express-engine
```

渲染项目：
```
npm run dev:ssr
```