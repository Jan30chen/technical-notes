# JavaScript-Dom操作

> 参考：https://juejin.im/post/6844903593011576845

## 获取Dom

### ID获取

```javascript
document.getElementById(id)
```

```javascript
const mainDom = document.getElementById('main');
const contentDom = document.getElementById('content');
```

### class类获取

```javascript
element.getElementsByClassName(class) // ie9+
element.querySelectorAll(.class) // ie8
```

```javascript
const mainDom = document.getElementById('main');
const infoDomList1 = mainDom.querySelectorAll('.info.test');
const infoDomList2 = mainDom.getElementsByClassName('info test');
```

### tag标签获取

```javascript
element.getElementsByTagName(tag)
```

```javascript
const divDom = document.getElementsByTagName('div');
const pDom = divDom[0].getElementsByTagName('p');
```

### name属性获取value（表单中）

```javascript
form-id.name.value
```

### 属性获取

```javascript
// querySelector返回返回的是单个DOM元素，querySelectorAll返回NodeList
element.querySelector //ie8+
element.querySelectorAll //ie8+
```

### 关系获取

#### 获取父元素

```
element.parentNode
```

#### 获取子元素

```
element.childNodes
```

#### 获取兄弟元素

```javascript
 element.previousSibling // 获取前一个兄弟节点或者null
 element.nextSibling     // 获取后一个兄弟节点或者null
```

## 操作Dom

### 创建Dom

```
document.createElement(tagName)
```

### 新增、插入Dom

#### 添加到父元素的最后

```
paranetElement.appendChild(child);
```

#### 添加到节点的前面

```
paranetElement.insertBefore(newElement, Element);
```

+ 通过`insertBefore`方法可以将`newElement`插入到`Element`前面，如果**`Element`是`null`**则将`newElement`插入到`paranetElement`的尾部
+ 如果`newElement`是一个已经存在在文档中的`DOM`,`insertBefore`则会表现为**移动该`DOM`**(将会保留所有的事件)

#### 添加到节点的后面

```
parentDiv.insertBefore(sp1, sp2.nextSibling);
```

+ 将`sp1`插入到`sp2`的后一个节点前，即添加到`sp2`节点后
+ 如果`sp2`没有下一个节点，则它肯定是最后一个节点，则`sp2.nextSibling`返回`null`，则`sp1`被插入到子节点列表的最后面（即`sp2`后面）

### 修改Dom

#### 获取或修改标签内容

```javascript
Element.innerHTML // 获取标签内的所有内容
Element.innerText // 只获取标签内的文字内容，不包括标签
```

#### 修改css样式

```
element.style.cssAttribute()
```

#### 修改属性

```javascript
element.setAttribute(name, value)
element.removeAttribute(name)
element.className //当前元素的class属性的值,可以是由空格分隔的多个class属性值.
```

#### 删除节点

```
paranetElement.removeChild(element)
```

+ 通过遍历该函数，清空子节点