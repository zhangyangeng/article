# 介绍

- 参考教程：
  - 官方[文档](https://www.typescriptlang.org/docs/handbook/basic-types.html)
  - 阮一峰[教程](http://ts.xcatliu.com/)
  - 尚硅谷[教程]()

# 概念

- 什么是 TS？
  - 是添加了类型系统的 JS，适用于任何规模的项目
  - 是一门静态类型、弱类型的语言
  - 是完全兼容 JS 的，它不会修改 JS 运行时的特性
- TS 特点：
  - TS 需要先编译为 JS，然后运行在浏览器、Node.js 等任何能运行 JS 的环境中
  - 拥有很多编译选项，可自由决定类型检查的严格程度
  - 可以和 JS 共存
  - 增强了编辑器（IDE）的功能，提供了代码补全、接口提示、跳转到定义、代码重构等能力
  - 拥有活跃的社区，大多数常用的第三方库都提供了类型声明
  - 与标准同步发展，符合最新的 ES 标准
- 核心设计理念：在完整保留 JS 运行时行为的基础上，通过引入静态类型系统来提高代码的可维护性，减少可能出现的 bug
- TS 与 JS 的不同点：
  - TS 是静态类型，它在运行前需要先编译为 JS，而在编译阶段就会进行类型检查；TS 坚持与 ES 标准同步发展；TS 非常适用于大型项目
  - JS 是动态类型，它在运行时才会进行类型检查
- TS 与 JS 的相同点：
  - 它们都是弱类型，允许隐式类型转换

## 开发环境

- 下载并安装 Node.js

- 使用 npm 全局安装 TypeScript：
```
npm install -g typescript
```

- 创建一个 ts 文件
- 编译一个 ts 文件：
```
tsc hello.ts
```

## 编辑器

- 推荐使用 VSCode

# 基础内容

## 1.类型声明

- 使用 `:` 声明变量的类型，`:` 的前后有没有空格都可以
```ts
// 给变量指定类型
let a : number = 10;
// 给函数的参数指定类型
function sum(a : number, b : number){
	return a + b;
}
// 给函数的返回值指定类型
function sum(a: number, b: number): number{
    return a + b;
}
let result = sum(123, 456);
```

- 如果变量的声明和赋值是同时进行的，ts 可以自动对变量进行类型检测
- 即使指定不符合的变量类型，执行 ts 文件时依然会生成 js 文件；如果要在报错的时候终止 js 文件的生成，可以在 `tsconfig.json` 中配置 `noEmitOnError` 即可

## 2.数据类型

### (1).number

- 表示数值类型
```ts
let a: number = 1;
```

### (2).string

- 表示字符串类型
```ts
let a: string = "a";
```

### (3).Boolean

- 表示布尔类型
```ts
let a: boolean = true;
```

- 注意，使用构造函数 `Boolean` 创造的对象**不是**布尔值，其返回的是一个 `Boolean` 对象

```ts
let createdByNewBoolean: Boolean = new Boolean(1);
```

### (4).|

- 联合类型表示取值可以为多种类型中的一种

- 当使用字面量进行类型声明时，是无法修改其值的
```ts
let a: 10;
```

- 这时我们可以使用联合类型 `|` 来连接多个类型
```ts
let a: "male" | "female";
let a: string | number;
```

- 当使用联合类型时我们**只能访问此联合类型的所有类型里共有的属性或方法**

### (5).any

- 表示任意类型，当一个变量设置为 any 相当于关闭了对该变量的 ts 类型检测
- 声明变量如果不指定类型，则 ts 解析器会自动判断变量的类型为 any（隐式any）
- <strong style="color:red;">当把一个 any 类型的值赋值给其他变量时不会报错，不建议使用</strong>

### (6).unknown

- 表示未知类型的值，是类型安全的 any
- <strong style="color:red;">当把一个 unknown 类型的值赋值给其他变量时会报错，建议使用</strong>
- 解决报错1：进行类型判断
```ts
let a: unknown;
a = "hello";
let s: string = "world";
if(typeof a === "string"){
    s = a;
}
```

- 解决报错2：类型断言
```ts
let a: unknown;
a = "hello";
let s: string = "world";
s = a as string;
s = <string>a;
```

### (7).void

- 表示空，在函数中表示没有返回值
```ts
function fn(): void{
    xxx
}
```

- 声明 void 类型的变量只能赋值给 undefined 和 null，而不能赋值给 `number` 类型

### (8).undefined & null

- 可以使用其来定义这两个原始数据类型
- `undefined` 和 `null` 是所有类型的子类型，所以其可以赋值给 `number` 类型的变量
```ts
let u: number = undefined;
```

### (9).never

- 表示永远不会返回结果，用于进行报错
```ts
function fn2(): never{
    throw new Error("报错了！");
}
```

### (10).object

- 表示对象，使用 `{}` 指定对象中可以包含哪些属性
- 属性名后加 `?` 表示属性是可选的
```ts
let b: {name: string, age?: number};
b = {name: "孙悟空"};
```

- 也可以使用 `[propName: string]: any` 表示任意类型的属性
```ts
let c: {name: string, [propName: string]: any};
c = {name: "孙悟空", age: 18}
```

### (11).function

- 使用箭头函数来设置函数的类型声明
```ts
let d: (a: number, b: number) => number;
d = function(a, b){
    return a + b;
}
```

### (12).array

- 表示数组，数组表示法主要有以下几个：

- 类型+方括号：
```ts
let e: string[];
e = ["a", "b"];
```

- 数组泛型
```ts
let f: Array<number>;
f = [1, 2];
```

- 接口：不常用，主要用来表示类数组
```ts
// 只要索引的类型是数字时，那么值的类型必须是数字
interface NumberArray{
	[index: number]: number
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5]
```


### (13).tuple

- 表示元组，元组就是固定长度的数组
```ts
let g: [string, string];
g = ["a", "b"];
```

### (14).enum

- 表示枚举
```ts
enum Gender{
    male = 0,
    female = 1
}
let h: {
    name: string,
    gender: Gender
}
h = {
    name: "孙悟空",
    gender: Gender.male
}
```

## 3.类型推论

- 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型
- 例如：
```ts
let a = "a";
// 等价于
let a: string = "a";
```

## 4.类型别名

- 当我们需要很多类型时，可能会因为太长而增加书写量，所以可以给类型设置别名
```ts
type myType = string | number | boolean;
let a1: myType;
let a2: myType;
let a3: myType;
a1 = "a";
a2 = 1;
a3 = true;
```

## 5.编译选项

### (1).自动编译

- 可以监视 ts 文件的变化，自动编译，在命令行中执行命令即可
```
tsc app.ts -w
```

- 缺点：只可以监视当前文件，且无法关闭监视窗口

### (2).全部编译

- 当我们想要编译全部 ts 文件时，需要现在该目录的根目录中创建 ts 的配置文件 —— `tsconfig.json` 
- 在命令行中使用 `tsc/tsc -w` 命令即可

#### 引申：配置文件

- 配置文件中以 `{}` 为最外部，其中主要有以下几个参数
- `include`：表示需要编译的目录或文件
  - `**` 表示任意目录
  - `*` 表示任意文件
- `exclude`：表示不需要编译的目录或文件
  - 默认值有：`["node_modules", "bower_components", jspm_packages]` 
- `extends`：表示继承哪个配置文件的配置
- `files`：表示需要编译的文件
- `compilerOptions`：表示编译器的选项
  - `target`：指定 ts 编译时 IE 版本
  - `module`：指定要使用的模块规范
  - `lib`：指定项目中要使用的库，一般不需要修改，只有在非浏览器哦环境下运行时才会需要改
  - `outdir`：指定编译后文件所在目录
  - `outfile`：将代码合并为一个文件
  - `allowJs`：是否对 js 文件进行编译，默认为 false
  - `removeComments`：是否在编译的时候移除注释
  - `noEmit`：不生成编译后的文件，默认为 false
  - `noEmitOnError`：当有错误时不生成编译文件
  - `strict`：所有的严格检查的总开关，即控制以下4个，建议设置为 true
  - `alwaysStrict`：设置编译后的文件是否使用严格模式
  - `noImplictAny`：不允许隐式 any 类型
  - `noImplictThis`：不允许不明确类型的 this
  - `strictNullChecks`：严格的检查空值

## 6.使用Webpack打包

### (1).安装相关依赖

- 在根目录中初始化package包
```cmd
npm init -y
```

- 在项目根目录中安装如下依赖；
```cmd
npm i -D webpack webpack-cli typescript ts-loader
```

- 依赖分别为：Webpack包、Webpack命令行工具、TypeScript包、ts编译器

### (2).创建webpack配置文件

- 在根目录中创建 `webpack.config.js` 文件
- 根据以下内容配置：
```javascript
// 引入一个包
const path = require("path");
// webpack 中的所有配置信息都应该写在 module.exports 中
module.exports = {
    // 指定入口文件
    entry: "./src/index.ts",
    // 指定打包文件所在目录
    output: {
        // 指定打包文件的目录
        path: path.resolve(__dirname, "dist"),
        // 打包后文件的名字
        filename: "bundle.js"
    },
    // 指定webpack打包时要用的模块
    module: {
        // 指定要加载的规则
        rules: [
            {
                // 指定规则生效的文件
                test: /\.ts$/,
                // 指定要使用的loader
                use: "ts-loader",
                // 指定要排除的文件
                exclude: /node_modules/
            }
        ]
    },
    // 配置webpack插件
    plugins: [
        new CleanWebpackPlugin(),
        new HTMLWebpackPlugin({
            title: "这里可以自定义标题",
            template: "./src/index.html"
        }),
    ],
    // 设置引用模块
    resolve: {
    	extensions: [".ts", ".js"]
    }
}
```

### (3).创建ts配置文件

- 在项目根目录中创建一个 `tsconfig.json` 文件
- 根据以下内容配置：
```json
{
    "compilerOptions": {
        "module": "ES2015",
        "target": "ES2015",
        "strict": true
    }
}
```

### (4).修改package.json文件

- 在 `scripts` 字段中添加 `build` 字段
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
},
```

### (5)自动生成HTML网页

- 在项目根目录的命令行中执行 `npm i -D html-webpack-plugin` 
- 在 `webpack.config.js` 中引入并配置：
```JavaScript
const HTMLWebpackPlugin = require("html-webpack-plugin");
module.exports = {
    // 配置webpack插件
    plugins: [
        new HTMLWebpackPlugin({
            title: "这里可以自定义标题",
            template: "./src/index.html"
        }),
    ]
}
```

### (6)Webpack开发服务器

- 在项目根目录的命令行中执行 `npm i -D webpack-dev-server` 
- 在 `package.json` 中的 `scripts` 字段中配置如下：
```json
	"start": "webpack serve --open chrome.exe"
```

- 优点：可以实时更新，即可以监控 ts 文件是否修改然后重新编译

### (7).编译时删除旧编译文件

- 在项目根目录的命令行中执行 `npm i -D clean-webpack-plugin`
- 在 `webpack.config.js` 中引入并配置：
```javascript
const {CleanWebpackPlugin} = require("clean-webpack-plugin");
module.exports = {
    plugins: [
        new CleanWebpackPlugin(),
    ]
}
```

# 面向对象

## 1.类

- 

## 2.构造函数

- 构造函数会在对象创建的时候调用，在实例方法中，this就表示当前的实例
- 因此可以通过 this 向新建的对象中添加属性

## 3.继承

- 通过继承可以将多个类中共同的代码写在一个父类中，所有的子类将拥有父类中的属性和方法
- 如果在子类中写了构造函数，则必须对父类的构造函数进行调用，即使用 `super()` 

## 4.抽象类

- 以 abstract 开头的类是抽象类，抽象类不能用于创建对象，所以抽象类是专门用来做继承的
- 在抽象类中可以定义抽象方法，抽象方法使用 abstract 开头且没有方法体，子类必须对抽象方法进行重写
```ts
sabstract class Animal{
    name: string;
    abstract sayHello(): void;
}
class Cat extends Animal{
    sayHello(){
        console.log("喵喵喵");        
    }
}
```

## 5.接口

- 接口用来定义一个类的结构，用来定义一个类中应该含有哪些属性和方法
- 接口也可以当做类型声明去使用
```ts
interface myInterface{
    name: string;
    age: number;
}
```

- 接口可以在定义类的时候去限制类的结构，且接口中所有的属性都不能有实际的值，接口中的方法都是抽象方法
- 定义类时，可以实现一个接口：
```ts
class MyClass implements myInterface{
	xxx
}
```

## 6.属性的封装

- 类的修饰符有三种：
  - public：公有属性，可以在任意地方访问与修改
  - private：私有属性，只能在类内部进行访问修改
  - protected：受保护的属性，只能在当前类和当前类的子类中使用
- 类中定义的属性与方法，如果不加以限制会被外界随意读取与修改，这并不是合规的，所以我们需要对属性进行相关的封装

### (1).js中属性的封装

- 在 js 中我们需要设置属性的修饰符为 `private`
- 然后使用属性的**存取器**来进行读取和设置：
```js
class Person{
    private name: string;
    private age: number;
    constructor(name: string, age: number){
        this.name = name;
        this.age = age;
    }
    getName(){
        return this.name;
    }
    setName(value){
        this.name = value;
    }
}
const p = new Person("孙悟空", 18);
p.setName("猪八戒");
console.log(p.getName());
```

### (2).ts中属性的封装

- 在 ts 中我们需要设置属性的修饰符为 `private`
- ts 中简化了操作，设置 getter 与 setter 的方法如下：
```ts
class Animall{
    private _name: string;
    private _age: number;
    constructor(name: string, age: number){
        this._name = name;
        this._age = age;
    }
    get name(){
        return this._name;
    }
    set name(value){
        this._name = value;
    }
}
const a = new Animall("小狗", 3);
a.name = "小猫";
console.log(a.name);
```

- 在 ts 中我们可以直接使用 `对象.属性` 的方式来读取或者修改属性值

## 7.泛型

- 在定义函数或类时，如果遇到类型不明确时就可以使用**泛型**
```ts
function fn<T>(a: T): T{
    return a;
}
fn(10);                 // 不指定泛型，TS可以自动进行类型推断
fn<string>("hello");    // 指定泛型
```

- 泛型也可以指定多个
```ts
function fn2<T, K>(a: T, b: K): T{
    return a;
}
fn2<number, string>(123, "hello");
```

- 泛型可以实现接口
```ts
interface Inter{
    length: number
}
function fn3<T extends Inter>(a: T): number{
    return a.length
}
```

- 泛型可以自动识别类型
```ts
class MyClass<T>{
    name: T;
    constructor(name: T){
        this.name = name;
    }
}
const mc = new MyClass<string>("孙悟空");
```

# 错误代码

## 错误1：

- 错误代码：`String does not match the pattern of "^(?:@[a-z0-9-*~][a-z0-9-*._~]*/)?[a-z0-9-~][a-z0-9-._~]*$".`
- 错误原因：package.json 文件中的名字必须全为小写

