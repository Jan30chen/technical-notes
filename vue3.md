# vue3

ref()：传入初值，返回一个响应式的ref对象
优点：可以监听对象深层；传递给函数时不会丢失其响应性
返回一个响应式对象，
api中使用param.value的形式获取；组件模板中会被自动解包，所以可以直接使用

reactive()：生成对象的响应式代理，可以不使用value而直接使用param；不等于源对象；深层地转换对象
局限：只能用于对象类型，不能用于原始类型；不能替换整个对象，不能被解构：否则丢失响应性

解包细节：ref作为响应式对象的属性被访问/修改时，自动被解包
响应式对象需要为深度响应式；
ref对象作为集合类型（数组）元素被访问时，不会被解包
模板中：最顶级的ref属性才会被解包；深层属性需要value属性
但如果作为最终计算值使用（没有附加的计算），也将被解包
const count = ref(0)、const obj = { id: ref(1)}：count、obj顶级属性，object.id不是
{{ count + 1 }}、{{ object.id }}：前者是顶级属性，后者为最终计算值，正确渲染
{{ object.id + 1 }}：错误，非顶级属性，且附加计算
解决：先解构为顶级属性再使用
const {id} = obj、{{ id+1}}

computed()
计算属性：使用computed方法生成，也为ref对象
计算属性与方法：前者在其依赖的响应式对象更新时重计算；后者总是在重渲染时再次执行
可写计算属性：提供get与set函数

动态类名：:class="{active: isActived}" => 直接绑定对象（计算属性）、绑定数组
组件的class会被附加在根元素上，多个根元素的情况，使用组件的$attrs属性指定

<p :class="$attrs.class"></p>

<u>浅层响应式是什么？</u>
只有对象本身具有响应式，而对象中的元素不具有响应式

通过shallow ref返回浅层的响应对象

toRaw()：根据代理返回原始对象

setup()钩子函数：组件中使用组合式API入口；
单文件使用`<script setup>`语法糖：不用手动去暴露setup中的对象
第一个参数是响应式props，如果解构会丢失响应性
第二个参数为上下文对象，对象为非响应，其中含有attrs、slots、emit等响应式对象；还有expose方法（模板引用时，只能获取该函数返回的对象）
defineExpose：当组件为`<s setup>`时，默认私有，不能使用ref去使用模板引用；使用该方法导出可引用的对象
可返回渲染函数
组合式API是什么？：相对于选项式（Vue2）的一种写法

import生命周期钩子函数：
onMounted()

$attr：组件透传 attributes 的对象
透传attributes：传递给一个组件，但没有被声明为 prop 或自定义事件（v-on）的属性；常见如class style id
子组件根元素存在class、style或v-on事件时，将会继承父组件的这些部分。不包含被 prop 命名和为emit声明的事件。

defineOptions是什么？

CSS 中的 v-bind()：传递变量至css中

Proxy对象

watchEffect(onCleanup)：立即运行函数，并响应式地追踪其中的依赖，依赖改变时重新执行
onCleanup：在侦听器重新调用或停止前，被执行，应该用来清除上次的副作用（如未完成的异步回调、设置的settimeout）
返回值为，用来停止该侦听器的函数

主作用与副作用(side effect)：
执行代码构建dom为主作用：相应的，用户操作导致的变化，为副作用

watch：可以监听ref、computed、响应式对象（reactive）、getter函数；以及包含以上元素的数组。
不能直接监听reactive元素的属性，改为返回一个该属性的getter函数
watch传入一个reactive对象，隐式地创建深层侦听器：所有嵌套的属性更改都会触发；而getter只有在本身整个被替换时才会触发。
watch与watchEffect：watchEffect为自动收集依赖项并侦听，且默认immediate的watch；在组件第一次挂载（watchEffect被创建时）被触发，watch默认第一次不触发；
当响应式状态同时导致了组件更新与侦听器回调，顺序为 回调 > 组件更新；使用flush: 'post'属性在组件更新后调用回调；
一般情况下不需要手动销毁侦听器，除非是异步侦听器；在考虑异步侦听器前，尝试条件式的同步侦听器

`<script setup>`语法：单文件组件使用组合式API时的语法糖
普通的`<script>`只在组件被首次引入执行，该语法在每次组件被创建时执行
所有顶层的绑定：变量、函数声明，import内容，可以直接在模板中使用
响应式变量需要明确使用ref等响应式API声明
组件引用：直接导入使用，使用动态组件，使用命名空间
defineProps 和 defineEmits：显式指定prop与emits的元素及其类型；不需要导入
s-setup组件默认不能被模板引用或$parent链获取其实例中的声明，如果需要，使用defineExpose显示地指定需要暴露的属性。暴露的ref会被自动解包。
defineOptions 声明组件选项


自定义指令

v-model在自定义组件中的使用
`<CustomInput v-model="searchText" />`
实际被解释为：

```vue
<CustomInput
  :modelValue="searchText"
  @update:modelValue="newValue => searchText = newValue"
/>
```

自动生成属性modelValue、update:modelValue；子组件中需要手动定义对应的prop与emit
可以自定义传入变量名与方法名：v-model:first-name；于是子组件中prop > firstName、emit > update:firstName
可以增加自定义修饰符，将会作为prop的modelModifiers对象中的属性被传入；如v-model.capitalize，子组件中使用modelModifiers.capitalize获取。
同时存在自定义变量与修饰符的情况下：v-model:title.capitalize：prop = ['title', 'titleModifiers']，emit = ['update:title']