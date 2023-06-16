# React

## 事件处理

### 传递参数

可以使用：

+ 箭头函数，其中事件对象`event`需要作为第二个参数*显式传入*
+ bind函数

```react
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

## Dom

> 参见：https://zh-hans.reactjs.org/docs/dom-elements.html

`ReactDom`上的属性，与原生html的属性有些不同，这些属性都为**小驼峰命名**

若使用用户自定义的属性，请使用全小写命名

+ `checked`：用于`checkbox`或`radio`类型的`<input>`组件，设置该受控组件的选中状态，使用变量设置该属性来切换组件状态
  + `defaultChecked`：对于不在上述范围内的非受控组件，设置首次挂载时的选中状态
+ `className`：替换`class`属性
+ `forHtml`：替换`for`属性
+ `onChange`：表单字段发生变化时，触发该事件
+ `style`：等同`style`属性，不同在于，其接受一个**小驼峰命名**属性的 JavaScript 对象
  + **不推荐**直接使用该属性，而是定义`className`，并于CSS文件中定义样式

定义对象，然后插入`style`属性

```react
const divStyle = {
  color: 'blue'
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```

或直接插入，此时外面的花括号表示这是一个变量属性，里面的花括号形成一个对象(Object)

```react
function HelloWorldComponent() {
  return <div style={{ color: 'blue' }}>Hello World!</div>;
}
```

+ `value`：受控组件的值，如`<input>`、`<select>` 和 `<textarea>` 组件
  + `defaultValue` 属性对应的是非受控组件的属性，用于设置组件第一次挂载时的值
+ `selected`：`<option>`组件的属性，指示元素选中状态；但是一般采用在根元素`<select>`上使用`value`属性来操纵选中状态

定义值，并添加变化时执行的函数

```react
<select value={this.state.value} onChange={this.handleChange}>
    <option value="grapefruit">葡萄柚</option>
    <option value="lime">酸橙</option>
    <option value="coconut">椰子</option>
    <option value="mango">芒果</option>
</select>
```

可以多选

```react
<select multiple={true} value={['B', 'C']}>
```

## 表单

React中的[表单](https://zh-hans.reactjs.org/docs/forms.html#the-select-tag)，可以使用state来更方便地管理与使用值，所以会和原生表单有些不同

### 受控组件

将表单元素的值，通过`value`属性保存在`state`中，并只能通过`setState`函数更新值；这样被接管原生的元素功能，使`state`成为唯一数据源的就是**受控组件**

例如`<input>`、`textarea`与`<select>`组件

`<input type="file" />`组件是一个特例，其值为只读的

### 多个输入

当存在多个输入元素时：

+ 可以使`onChange`事件绑定不同的回调函数
+ 添加不同的`name`属性，于同一个回调函数中获取`target.name`来分辨不同的元素

### 阻止输入

通过设置`value`属性为常量，阻止用户对其进行修改

但若被设置为`undefined`或`null`，则依然可以被修改

### 非受控组件

为元素添加值属性与回调函数，有时可能略显繁琐；

此时，可以直接使用原生Dom元素（即[非受控组件](https://zh-hans.reactjs.org/docs/uncontrolled-components.html)），并[使用ref](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html)来获取该元素的相关信息：

+ 使用 `React.createRef()` 创建 ref 类型的变量
+ 将 ref 变量赋值给元素上的`ref`属性

## 条件渲染

[条件渲染](https://zh-hans.reactjs.org/docs/conditional-rendering.html)有以下几种方法：

+ 创建两种情况下的组件A与B，并创建组件C，在C中根据条件决定返回需要返回的组件；之后渲染C组件即可
+ 创建有状态组件，在组件的 JSX 中使用 if 语句
  + 更简洁的方法，使用三元运算符
  + 如果控制单个组件的渲染与否，可以使用与运算符`&&`，当**条件符合时，不渲染该组件**
  + 控制单个组件，也可以直接返回`null`使其不渲染

无状态组件方法：

```react
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}
```

有状态组件方法：

```react
//	插入变量
render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
        button = <LogoutButton />;
    } else {
        button = <LoginButton />;
    }

    return (
        <div>
            <Greeting isLoggedIn={isLoggedIn} />
            {button}
        </div>
    );
}

//	直接插入
return (
    <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {
            if (isLoggedIn) {
                <LogoutButton />
            } else {
                <LoginButton />
            }
            //	或
            isLoggedIn ? <LogoutButton />:<LoginButton />
            //	控制显隐
            !isRender ? <PrimaryButton />
        }
    </div>
);

//	返回null阻止渲染
function WarningBanner(props) {
  if (!props.warn) {
    return null;	//	不会渲染
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```

## 列表渲染

基于一个列表，渲染元素列表，使用`map`函数来遍历，同样有两种方法：

+ 声明一个 JSX 对象，赋值后插入到需要的地方
+ 直接在需要的地方，使用`{}`插入遍历代码，生成元素列表

### key

被遍历渲染的元素项，需要添加`key`属性，提供唯一的标识，帮助 React 识别重渲染时被修改的元素；

一般可以使用元素 id ，当没有被指定时，默认使用元素索引`index`，并会打印警告。

`key`属性添加在被遍历的元素上，也就是`map`函数循环渲染的*最外层元素*

---

`key`属性虽然被添加在元素上，但并不会向下传递，也就是其中的子元素不能获取到`props.key`；需要`key`的情况下，使用另一个属性名传递相同的值

