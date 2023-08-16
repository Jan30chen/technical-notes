# Other-注释

> 参考：[前端代码注释](https://juejin.cn/post/6844903789426638862)、[注释那些事儿 - 前端代码质量系列文章（一）](https://juejin.cn/post/6844903593825452039)
>

## 原则

添加代码注释需要遵循以下两个原则：

1. 不在不需要的地方添加注释，避免造成注释冗余
2. 在需要添加注释的时候，尽可能详细

需要添加注释的地方：

+ 描述文件
+ 描述类
+ 组件暴露的公共方法、工具类方法和业务实现的关键方法
+ 关键变量、关键常量、`TODO`和约定的特殊标记
+ 复杂业务的实现逻辑

## 文件注释

文件头部的注释，用于快速了解该文件的功能

包含以下字段：

- `@description`：文件描述
- `@author`：作者
- `@date`：创建时间
- `@lastModifiedBy`：最新的变更人
- `@lastModifiedTime`：最新的更新时间

```js
/**
 * @description 文件干啥用的
 * @author CaiCai
 * @date 2019-2-20 16:16:52
 * @lastModifiedBy CaiCai·
 * @lastModifiedTime  2019-2-25 20:07:43
 */
```

## 类注释

放在类声明代码之上

+ `@description`：一个参数描述类的说明。
+ `@property`：描述public的实例属性。有三个参数：
  + 第一个参数表示属性的数据类型，用 {} 包含
  + 第二个参数表示属性名
  + 第三个参数用来表示属性的说明。 属性名和属性说明用‘ - ’连字符来连接，连接符前后需要空格
+ `@hideconstructor` ：**无参数**，表示该类隐藏了构造函数。即定义中无构造函数，使用默认构造函数。有构造函数时，则不能使用该标识
+ `@param`：描述创建实例时的“参数”：有三个参数：
  + 第一个参数表示“参数”的数据类型，用 {} 包含。
  + 第二个参数表示“参数”名。
  + 第三个参数表示“参数”的说明。 “参数”和“参数”说明用‘ - ’连字符来连接，连接符前后需要空格
+ `@extends`：描述类继承自那个父类
+ `@static`：标识类的静态方法

```js
/**
 * @description 一个参数用来描述类的主要功能
 * @property {string} props - 类的属性
 * @param {string} arg - 创建实例时的参数
 * @extends Parent
 */
class Test extends Parent {
    /**
     * @description 这是个静态方法
     * @returns { void }
     * @static
     */
    static testStaticMethod() {
    }
    constructor(arg) {
        super()
        this.props = arg
    }
}
```

## 方法注释

放在函数声明代码之上

+ `@description`：一个参数用来描述方法说明。
+ `@param`：描述创建实例时的“参数”。有三个参数：
  + 第一个参数表示“参数”的数据类型，用 {} 包含。
  + 第二个参数表示“参数”名。
  + 第三个参数表示“参数”的说明。
    +  “参数”和“参数”说明用‘ - ’连字符来连接，连接符前后需要空格。
    + 如果“参数”是个对象则用多个`@param`来声明。 “参数”名用[]括起来表示该参数是可选参数，[]内可以用=设置默认参数。
    + “参数”是个数组时，数据类型后添加[]。
+ `@returns`：标识方法的返回值 包含两个参数：
  + 第一个参数是返回的数据类型，可返回多种数据类型，用|分割。
  + 第二个参数是返回值的说明。
+ `@example`：通过例子来解释方法怎么用。

```js
/**
 * @description 保存用户信息
 * @param { object } user - 用户
 * @param { string } user.name - 用户名
 * @param { number } user.age - 年龄
 * @param { boolean } [user.isMarried = false] - 是否已婚
 * @param { object[] } [exGirlFriends] - 前女友
 * @param { string } exGirlFriends[].name 前女友名字
 * @param { number } exGirlFriends[].age 前女友年龄
 * @param { number } [id] - 用户ID
 * @returns { (number|string) } 用户ID
 * @example
 * updateUserInfo({name:'CaiCai',age:26,isMarried:true})
 * // => 1
 */
function updateUserInfo() {
    
}
```

## 单行注释

以`//`开头的单行注释，注意点：

+ `//`与注释内容加一个空格
+ 不追加到代码后，而置于被注释代码上
+ 注释行上添加一个空行，除非是块的顶部

用于关键变量、常量，TODO和FIXME等约定的特殊标识

```js
var active = true;	// 不好

// 好
var active = true;

// FIXME 此处有个问题尚未解决
// TODO 此处需要追加做点什么
```

## 多行注释

用`/*`开始，用`*/`结束的多行注释，注意点：

+ 开头结尾的`*`对齐
+ 注释写在之间，不与标识符同行

```js
/*
 这是多行注释，可以用来说明：
 组件暴露的公共方法、工具类方法和业务实现的关键方法；
 复杂业务的实现逻辑。
 */
```

---

文件、类和方法的注释形式可以稍作增加

```js
/**
 * @description: 
 * @param {*} aaa
 * @return {*}
 * @example: 
 */
```

## JSDoc

> 参见：[JSDoc中文文档](https://www.jsdoc.com.cn/)

JSDoc注释通常应该放在记录代码之前。为了被 JSDoc 解析器识别，每个注释必须以 `/**` 序列开头。以 `/*`、`/***`开头或超过3颗星的注释将被忽略。这个特性用于控制解析注释块的功能。

## 注释插件推荐

[Better Comments](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)：让注释按颜色区分不同的类别

