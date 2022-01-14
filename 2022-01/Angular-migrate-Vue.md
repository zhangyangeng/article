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
| 初始值 | 子组件接收时可以设置初始值 | 子组件接收时不可设置初始值 |

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

`:deep(<inner-selector>)`

