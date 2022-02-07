# Class 导入方式

Angular：
- 在 ts 文件中 `export class ModelOperationComponent implements OnInit, OnDestroy`

Vue：
- 在 vue 文件的 `<script>` 标签中 `export default class ModelOperation extends Vue` 

# 关于构造函数

Angular：
- 在 Angular 中是有构造函数的概念的，而一些服务是直接在构造函数中注入的
```ts
public constructor(private translateService: TranslateService) {
}
```

Vue：
- 在 Vue 中是没有构造函数的概念的，所以直接在导出的 class 中直接实例化即可
```ts
private modelOperationService = new ModelOperationService();
```

# 生命周期函数

| 钩子 | Angular | Vue |
|:--:|:--:|:--:|
| 初始化 | ngOnInit()/constructor() | created() |

# 国际化

Angular：
this.translateService.instant()

Vue：
(1)导入  import { useI18n } from 'vue-i18n';
(2)声明  private translate = useI18n();
(3)使用  this.translate.t(key);


# 服务中导出格式

Angular中：
```ts
@Injectable()
export class ModelOperationService {
}
```

Vue中：
```ts
import {HttpRequestService} from '@/apis/http-service';
export default class ModelOperationService extends HttpRequestService {
}
```

# 单例

一直都使用一个对象，然后存到一个Map里


# Class写法：

Vue：
- 文档：https://class-component.vuejs.org/
- 教程：
  - https://www.cnblogs.com/qingheshiguang/p/14696557.html
  - https://blog.csdn.net/baidu_39195199/article/details/115678096
  - https://www.jianshu.com/p/3cbcdd766295


# 问题

## 父子组件传值

子组件中 `@Prop public viewMode: string;` 不可以设置初始值

对比：
| 对比项 | Angular | Vue |
| :--: | :--: | :--: |
| 语法 | @Input()/@Output() | @Prop()/@Emit() |
| 初始值 | 子组件接收时可以通过 = 来直接赋值 | 子组件接收时不可不可直接设置初始值，需要在括号中使用 default 项来指明 |

装饰器仓库：[Github](https://github.com/kaorun343/vue-property-decorator)

## 日期格式化

Angular 中自带了 formatDate 日期格式化，而 Vue 中没有自带

Vue解决方法：
- `moment.js` —— vue-moment
- `day.js`
- 自己封装格式化逻辑

对比：
| 比较项 | Moment.js | Day.js | date-fns |
| :--: | :--: | :--: | :--: |
| 大小 | 16.7k（重） | 2.87k（轻） | 5.76k（轻） |
| 最后更新时间 | 2021.02（停止开发，仅维护） | 2021.09 | 2021.12 |
| 优势 | 支持number类型直接转换为时间对象 | 返回新的实例，不可变数据，支持链式操作，API与Moment一致，支持插件 | 模块化加载，适用复杂项目，不可变性和纯粹性 |
| 劣势 | 可变对象 | 不支持number类型直接转换 | 不支持全局导入 |
| 浏览器兼容 | FireFox与Safari有异常 | 全浏览器 | |
| Tree-shaking | 不支持 | 不支持 | 支持 |
| 模式 | OO | OO | 	Functional |
| 国际化 | 支持 | 支持 | 支持 |
| TypeScript | 支持 | 支持 | 支持 |
| License | MIT | MIT | MIT |
| 文档 | [中文](http://momentjs.cn/) | [中文](https://dayjs.gitee.io/docs/zh-CN/installation/typescript) | [外文](https://date-fns.org/docs/Getting-Started/) |
| 仓库 | [GitHub](https://github.com/moment/moment) | [GitHub](https://github.com/iamkun/dayjs) | [GitHub](https://github.com/date-fns/date-fns) |

参考链接：
- [【译】你可能不需要Moment.js](https://juejin.cn/post/6844903681029046280)
- [零散专题09 Moment.js和date-fns](https://duola8789.github.io/2017/04/26/01%20%E5%89%8D%E7%AB%AF%E7%AC%94%E8%AE%B0/07%20%E9%9B%B6%E6%95%A3%E4%B8%93%E9%A2%98/%E9%9B%B6%E6%95%A3%E4%B8%93%E9%A2%9809%20Moment.js%E5%92%8Cdate-fns/)
- [moment、dayjs、date-fns时间日期库比较](https://www.yht7.com/news/25869)

## Timer单位问题

在 ts 文件中如果想要使用定时器，需要给定时器设置一个类型，虽然定时器返回的是 number 类型，但是如果按 number 类型定义时会出错，所以需要借用 NodeJS.Timer 来进行定义
```ts
import Timer = NodeJS.Timer;
private timer: Timer;
```

这时会出现报错 `'NodeJS' is not defined.eslint`

解决办法参考该 [issue](https://github.com/Chatie/eslint-config/issues/45)

# Ant Design Vue

## 1.快速给指定标签设置样式

`:deep(<inner-selector>)`

注意：之前的 `::v-deep` 虽然可以使用，但会在控制台警告被废弃

## 2.DatePicker

该组件中可以通过 `disabled-date` 来限制可选择日期的范围，其通过箭头函数来返回需要限制的条件，官方写法如下：
```ts
const disabledDate = (current: Dayjs) => {
	// Can not select days before today and today
	return current && current < dayjs().endOf('day');
};
```

参数 `current` 指的是当前时间选择面板的日期，语句 `dayjs().endOf('day')` 指的是返回当前 dayjs 对象并设置为时间末尾

整个返回语句表明今天和比时间末尾小的时间都不可以选择

## 2.Table

[ant design 表格columns配置解析(text, record)](https://blog.csdn.net/weixin_41301816/article/details/121055948)

### 设置复选框

需要给表格设置复选框时，如果从后台接收的源数据没有key值，则需要给源数据中指定 key 值用于区分选中的是哪一行
```ts
// this.historyList 从后台获取的源数据
for (let i: number = 0; i < this.historyList.length; i++) {
    this.historyList[i].key = i;
}
```

## 3.Select

关于 placeholder 的显示，Angular 和 Vue 下是不同的

- 前者只需要将 value 为 `''` 即可显示 placeholder
- 而后者需要将 value 设置为 `undefined` 才可以正常显示 placeholder

VSCode插件：Ant Design Vue helper

# Vue数据变更检测不到

当 Vue 监测一个对象的时候，如果对象中的某一属性发生变化，Vue可能监测不到其变化，因为对象的引用地址并没有变化

因此，这个时候我们就需要手动强制更新模板来重新渲染

方法可参考 [这里](https://blog.csdn.net/qq449245884/article/details/104057886)

# 路由

## 1.携带参数跳转路由

点击[这里](https://www.cnblogs.com/ysx215/p/14990691.html)

## 2.query和params的区别

点击[这里](https://blog.csdn.net/weixin_44867717/article/details/109773945)

## 3.使用watch监听参数变化

点击[这里](https://next.router.vuejs.org/zh/guide/essentials/dynamic-matching.html#%E5%93%8D%E5%BA%94%E8%B7%AF%E7%94%B1%E5%8F%82%E6%95%B0%E7%9A%84%E5%8F%98%E5%8C%96)


# 控制台报错

## 1.Extraneous non-emits event listeners (viewModeChange, modelConfigChange) were passed to component but could not be automatically inherited because component renders fragment or text root nodes.

**报错信息**：`Extraneous non-emits event listeners (viewModeChange, modelConfigChange) were passed to component but could not be automatically inherited because component renders fragment or text root nodes.`

**报错原因**：子组件中的结构没有放在一个根容器中

**解决办法**：将子组件的结构放在一个div中即可
```html
<template>
    <div>
        ...子组件内容
    </div>
</template>
```

## 2.Property "options" was accessed during render but is not defined on instance

**报错信息**：`Property "options" was accessed during render but is not defined on instance`

**报错原因**：该组件上使用 v-model 绑定的属性没有定义初始值

**解决办法**：找到该属性值，根据其类型定义一个初始值
```ts
// 这里是使用了 vue-class-component 插件写的语法
public showSelectValues: Array<string> = [];
```

# TypeScript 报错

## 1.Parsing error: Unexpected token. Did you mean `{'>'}` or `&gt;`?

**报错信息**：`Parsing error: Unexpected token. Did you mean {'>'} or &gt;?`

**报错原因**：在该行代码处使用了标签形式的类型声明，而 ESlint 在检测的时候误以为 HTML 标签：
```ts
// 这里的<number>
this.corpusMaxNum = <number>res.paramList.filter(item => item.id === 'Titan.Model.Corpus.MaxNum')[0].value;
```

**解决办法**：使用 `as` 进行类型断言即可
```ts
this.corpusMaxNum = res.paramList.filter(item => item.id === 'Titan.Model.Corpus.MaxNum')[0].value as number;
```

# Vue 中使用Echarts

## v-for循环图表时只显示第一个图表

**错误现象**：当某个子组件专门封装了一个Echarts图表，在父组件中循环子组件时会出现只有第一个子组件可以正常渲染，剩下的子组件图表不正常显示

**错误原因**：因为在循环的时候 Echarts 图表的容器id都是一样的，导致 Vue 无法判断后面几个图表怎么渲染，所以就显示异常

**解决办法**：当使用 `v-for` 时，我们可以获取到index值，因此直接在id上使用 ES6 的模板字符串引入这个index值，同时在实例化 Echarts 时也这样即可正常显示
```html
<div class="chart" :id="`pieEcharts${index}`"></div>
const pieEcharts = echarts.init(document.getElementById(`pieEcharts${this.index}`));
```

## 2.使用resize()方法时报错

**报错信息**：`Cannot read properties of undefined (reading 'type')`

**报错原因**：Vue3中使用 proxy 的方式监听响应式，Echarts 的实例会在 Vue 内部转换成响应式对象，从而在执行 `resize()` 方法时获取不到 `coordSys.type` 

**解决办法**：在实例化 Echarts 时，将其指定为非响应式即可，如下：

```ts
const chartsInstance: echarts.ECharts = markRaw(echarts.init(document.getElementById('echarts')));
```

**原文链接**：源自 [@Bin_x](https://www.cnblogs.com/Bin-x/p/15342949.html)

# 待整理

## 1.调用子组件的方法及组件类型定义

通过 `@Ref()` 注解来定义子组件
```ts
@Ref('audio')
public audio: AudioPlay;
或
@Ref()
public audio: InstanceType<typeof AudioPlay>;
```

然后通过 this 直接调用即可
```ts
this.audio.pause();
```


## 2.路由问题


## 3.:deep问题

虽然在样式标签中使用了 `scoped` 来限制当前的样式只应用于当前的组件

但是当需要使用 `:deep` 来修改组件库样式（如 Ant Design Vue）时， 如果子组件也使用了和父组件一样的组件库时，父组件修改的样式就会覆盖到子组件上

所以需要在子组件的样式前面限定一样，这个样式只作用于某一容器中的组件中