# vue.js
`引用文献:`  
* [vue.js文档](https://cn.vuejs.org/v2/guide/installation.html)


# Vue实例  

## 创建一个Vue实例  
每个vue应用都是通过`Vue`函数创建一个新的Vue实例开始的：  
```javascript
var vm = new Vue({
    //选项
})
```  
虽然没有完全遵循[MVVM模型](https://zh.wikipedia.org/wiki/MVVM)，但是Vue的设计也受到他的启发。  

一个Vue应用有一个通过`new Vue`创建的**根Vue实例**，以及可选的嵌套的、可复用的组件数组成。  

```
根实例
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```  

## 数据与方法  
当一个Vue实例被创建时，它向Vue的响应式系统中加入了其`data`对象中能找到的所有的属性。当这些属性的值发生改变时，试图将会产生“响应”，即匹配更新为新的值。  
```javascript
//我们的数据对象
var data = { a:1}

//该对象加入到一个Vue实例中
var vm = new Vue({
    data: data
})

//获得这个实例上的属性
//返回源数据中对应的字段
vm.a == data.a //=> true

//设置的属性也会影响到原始数据 
vm.a = 2
data.a //=> 2

//
```
当这些数据改变时，视图会进行重渲染。值得注意的是只有当实例被创建时`data`中存在的属性才是响应式的。也就是说吐过你添加一个新的属性  

如果你知道你会在晚些时候需要一个属性，但是一开始他为空或不存在，name你需要设置一些初始值。  
```javascript
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```
这里唯一的例外是使用`Object.freeze()`，这会阻止现有属性，以为这想用系统无法再追踪变化。  
```javascript
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```
```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- 这里的 `foo` 不会更新！ -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

除了数据属性，Vue实例还暴露了一些有用的实例属性与方法。他们都有前缀`$`，以便与用户定义的属性区分开来。  
```javascript
var data = { a: 1}
var vm = new Vue({
    el: '#example',
    data: data
})

vm.$data === data //=> true
vm.$el === document.getElementById('example')// => true

//$watch 是一个实例方法
vm.$watch('a', function(newValue, oldValue){
    // 这个回调将在 `vm.a` 改变后调用
})
```

##实例的生命周期钩子  

么个Vue实例在被创建时都要经过一系类的初始化过程——例如，需要设置数据监听、编译模板、将实例挂在到DOM并在数据变化是更新DOM等。同时在这个过程中也会运行一些`生命周期钩子`函数,这给用户在不同的阶段添加自己代码的机会。  

比如 `created` 钩子可以用来在一个实例被创建之后执行代码：
```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
``` 

也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 `mounted`、`updated` 和 `destroyed`。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。
```
不要在选项属性或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 created: () => console.log(this.a) 或 vm.$watch('a', newValue => this.myMethod())。因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例，经常导致 Uncaught TypeError: Cannot read property of undefined 或 Uncaught TypeError: this.myMethod is not a function 之类的错误。
```  

# 模板语法  

vue.js使用了基于HTML的模板语法，允许开发者声明式的将DOM绑定至底层vue实例的数据。所有Vue.js的模块都是合法的HTML，所以能被遵循规范的浏览器和HTML解析器解析。  

在底层实现上，Vue将模板编译成虚拟DOM渲染函数。结合响应系统，Vue 能够智能的计算出最少需要重新渲染多少组件，并把DOM操作次数减少到最少。  

## 差值  

### 文本  

数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值：  
```html
<span>Message: {{ msg }}</span>
```  

Mustache 标签将会被替代为对应数据对象上的`msg`属性值。无论何时，绑定的数据对象上`msg`属性发生了改变，插值处3都会更新。  

通过使用 [v-once 指令](https://vuejs.bootcss.com/v2/api/#v-once)，也能执行一次性的插值，当数据改变时，插值处的内容不会更新。  
```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

### 原始HTML  

双的大括号将数据解析为普通文本，而非HTML代码。为了输出真正的HTML，你需要使用`v-html`指令：  
```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```  

### 特性  

Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 [v-bind ](https://vuejs.bootcss.com/v2/api/#v-bind)指令：
```html
<div v-bind:id="dynamicId"></div>
``` 

在布尔特性的情况下，它们的存在即暗示为 `true`，`v-bind` 工作起来略有不同，在这个例子中：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果 `isButtonDisabled` 的值是 `null` 、`undefined` 或 `false` ，则 disabled 特性甚至不会被包含在渲染出来的 `<button>` 元素中。  

### 使用JavaScript表达式  

迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。 
```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属Vue实例的数据作用于下作为JavaScript被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下边的例子都不会生效。  
```html
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

> 模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 `Math `和 `Date` 。你不应该在模板表达式中试图访问用户定义的全局变量。

## 指令  

指令（Dirctives）是带有 `v-` 前缀的特殊性。指令的值语气是单个 JavaScript表达式(v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。回顾我们在介绍中看到的例子：  

```html
<p v-if="seen">现在你看到我了</p>
```
这里，`v-if`指令将根据表达式`seen`的值的真假来插入/移除 `<p>`元素。  

### 参数  

一些指令能够接受一个"参数"，在指令名称之后以冒号表示。例如，`v-bind`指令可以用于响应式的更新HTML特性：  
```html
<a v-bind:href="url"></a>
```
在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` 特殊与表达式 `url` 的值绑定。另一个例子是 `v-on` 指令，他用于监听DOM的事件：  

```html
<a v-on:click="doSomething">...</a>
```

在这里参数是监听的事件名。我们也会更详细的讨论事件处理。

### 修饰符  

修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：  
```html
<form v-on:submit.prevent="onSubmit">...</form>
```  

## 缩写  

v- 前缀作为一种视觉提示，用来识别模板中 Vue 特定的特性。当你在使用 Vue.js 为现有标签添加动态行为 (dynamic behavior) 时，v- 前缀很有帮助，然而，对于一些频繁用到的指令来说，就会感到使用繁琐。同时，在构建由 Vue.js 管理所有模板的单页面应用程序 (SPA - single page application) 时，v- 前缀也变得没那么重要了。因此，Vue.js 为 v-bind 和 v-on 这两个最常用的指令，提供了特定简写：  

### v-bind 缩写  

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```  

### v-on 缩写  
```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```  

# 条件渲染

## `v-if`  

在字符串模板中，比如 [Handleebars](https://handlebarsjs.com/):  
```html
<!-- Handlebars 模板 -->
{{#if ok}}
  <h1>Yes</h1>
{{/if}}
```  

在Vue中，我们使用 `v-if`指令实现同样的功能：  
```html
<h1 v-if="ok">Yes</h1>
```

也可以用 `v-else` 添加一个"else块"：  

```html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```  

## 在 <template> 元素上使用 v-if 条件渲染分组  

因为 v-if 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素，此时可以把一个 template 元素当做不可见的包裹元素，并在上面使用 v-if 。最终的渲染结果将不包含 <template> 元素。
```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```  

v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则将不会被识别。  

### v-else-if  

v-else-if，顾名思义，充当 v-if 的“else-if 块”，可以连续使用：  
```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```  

类似于 `v-else` , `v-else-if` 也IXUS紧跟在带有 `v-if` 或者 `v-else-if` 的元素之后。

 
 # 列表渲染  
 
 ## 用 `v-for` 把一个数组对应为一组元素  

 我们用 v-for 指令根据一组数组的选项列表进行渲染。 v-for 指令需要使用 item in items 形式的特殊语法，items 是元数据租并且 item 是数组元素迭代的别名。  
 ```html
 <ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
 ```
 ```javascript
 var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
 ```

 在 `v-for` 块中，我们拥有对父元素作用域属性的完全访问权限。`v-for` 还支持一个可选的第二个人参数为当前向的索引。  

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```javascript
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```  

你也可以用 of 替代 in 作为分隔符，因为它是最接近 JavaScript 迭代器的语法：  
```html
<div v-for="item of items"></div>
```  

## 一个对象的 `v-for`  

你也可以用 v-for 通过一个对象的属性来迭代。  
```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```
```JS
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```

你也可以提供第二个人参数为键名：  
```HTML
<div v-for="(value, key) in object">
  {{ key }}: {{ value }}
</div>
```

第三个参数为索引：  
```html
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```

> 在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。

## key  

当Vue.js 用 `v-for` 正在更新以渲染过的元素列表是，他默认用“就地服用的策略”。如果数据项的顺序被改变，vue将不会移动DOM元素来匹配数据项的顺序，而是简单服用此处每个元素，并且却包他在特定索引下显示以被渲染够的每个元素，这个雷士Vue 1.x 的 `track-by="$index"`。  

这个默认的模式是高效的，但是只使用与不依赖子组件状态或临时 DOM 状态 (例如：表单输入值的类表渲染输出)。  

为了给Vue一个提示，以便他能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` 属性。理想的 `key`值是每项都有的唯一id。这个特殊的属性相当于Vue 1.xd的 `track-by`，但他的工作方式类似于一个属性，所以你需要用 `v-bind`来绑定动态值：  

```html
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```  

建议尽可能在使用 v-for 是提供 key ，除非遍历输出的 DOM 内容非常简单，或者是可以依赖默认行为以获取性能上的提升。  

因为他是vue识别节点的一个通用机制，key 并不与 v-for 特别关联，key 还具有其他用途  

