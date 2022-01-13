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

## 日期格式化

Angular 中自带了formatDate，而Vue中没有自带

Vue解决方法：
- `moment.js` —— vue-moment
- `day.js`
- 自己封装格式化逻辑
- 

## Timer单位问题


