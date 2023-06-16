# JavaScript-导入导出

> 参考：https://zhuanlan.zhihu.com/p/33843378

## import 和 require 的异同

### 不同比较

+ `import`
  + 属于ES6模板，是在ES6中才引入的功能，在此之前没有模板（module）体系
  + 输出的是值的引用，输出接口动态绑定，可以动态更新
  + 本质是解构过程，**编译时调用**，import语句会被提升至第一行（export语句也会被提升，但是赋值语句不会在原本位置运行，所以一开始导出`undefine`）
+ `require`
  + 属于CommonJS模板，是ES6之前，社区基于CommonJS规范提供的非正式方案
  + 生成输出对象的拷贝，因此输入对象中不能动态更新输出对象
  + 本质是赋值过程，**运行时调用**，必须在使用前进行声明

### 相同之处

**！模块不会重复执行**

无论是 ES6 模块还是 CommonJS 模块，当你重复引入某个相同的模块时，模块只会执行一次

+ `import`：因为是动态输出，随时都可以获取最新的引用，无需重复引用
+ `require`：第一次引用后将缓存对象，之后再次的引用都去读取缓存

## import 与 export 的配套使用

### import

```js
import defaultName from 'modules.js';			// 导入默认模板
import { export } from 'modules';				// 导入某命名模板
import { export as ex1 } from 'modules';		// 以别名导入某命名模板
import { export1, export2 } from 'modules.js';	// 导入多个模板
import { export1 as ex1, export2 as ex2 } from 'moduls.js';	// 以别名导入多个模板
import defaultName, { expoprt } from 'modules';	// 混合导入，默认和命名
import * as moduleName from 'modules.js';
import defaultName， * as moduleName from 'modules';
import 'modules';								// 运行模块中的全局代码，而不导入任何值
```

### export

```js
// 命名导出
export { name1, name2, ..., nameN };							// 导出命名模板
export { variable1 as name1, variable2 as name2, ..., nameN };	// 以别名导出命名模板
export let name1, name2, ..., nameN; 							// 直接导出变量，也可以使用var
export let name1 = ..., name2 = ..., ..., nameN; 				// 直接导出赋值过的变量，也可以使用var、const
export function FunctionName() {...}							// 直接导出函数
export class ClassName {...}									// 直接导出类
// 默认导出
export default expression;	// 默认导出，导入时可以用任何名字
// import anyName from ...;                                
export default function (...) { ... } 		// also class, function*
export default function name1(...) { ... } 	// also class, function*
export { name1 as default, ... };
// 模板重定向
export { foo, bar } from 'my_module'; 	// 等于导入my_module.js再导出 
export {default} from './other-module'; // 默认导出要加花括号
export * from ...;	// 导出全部并重定向	
export { name1, name2, ..., nameN } from ...;
export { import1 as name1, import2 as name2, ..., nameN } from …;
```

## 动态 import

### 定义

由于`import`于编译时调用，所以无法动态生成输入路径或判断输入不同的路径，此时可以使用动态`import`

1. 动态`import()`提供一个基于 Promise 的 API
2. 动态`import()`可以在脚本的任何地方使用
3. `import()`接受字符串文字，你可以根据你的需要构造说明符

```js
// a.js
const str = './b';
const flag = true;
if(flag) {
  import('./b').then(({foo}) => {
    console.log(foo);
  })
}
import(str).then(({foo}) => {
  console.log(foo);
})
```

### 其他使用技巧

+ 使用`Promise.all()`并行异步加载多个模板
+ 使用`Promise.race()`多个地址竞速加载同一个模板，保证加载最快的一个

## 其他

+ 指定模块文件的位置时候，可以是相对路径，也可以是绝对路径，`.js`后缀可以省略
+ 如果只是模块名，不带有路径，那么必须有配置文件，告诉 JavaScript 引擎该模块的位置。某些打包工具可以允许或要求使用扩展名。
+ `import`和`export`要成对使用