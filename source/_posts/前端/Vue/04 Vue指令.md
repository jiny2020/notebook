---
title: Vue 指令
date: 2021-05-26 12:12:57
categories: 
- 前端/Vue
tags:
- Vue 指令
---

### 一、什么是指令
指令 (Directives) 是带有 v- 前缀的特殊 attribute。指令 attribute 的值预期是单个 JavaScript 表达式 (v-for 是例外情况)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

### 二、常见指令
#### 1. v-if
1. v-if 指令将根据表达式的值的真假来插入/移除元素
2. 因为 v-if 是一个指令，所以必须将它添加到一个元素上，但是如果想切换多个元素时，可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 v-if。最终的渲染结果将不包含 `<template>` 元素。
3. 可以使用 v-else 指令来表示 v-if 的 else 块，v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。
4. v-else-if，顾名思义，充当 v-if 的 else-if 块，可以连续使用，类似于 v-else，v-else-if 也必须紧跟在带 v-if 或者 v-else-if 的元素之后。
5. 用 key 管理可复用的元素，Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染，添加一个具有唯一值的 key attribute，则不同key的元素不能复用。
6. v-show 只是简单地切换元素的 CSS property display，不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换，v-show 不支持 template 元素；而 v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建；一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
7. 不推荐同时使用 v-if 和 v-for，当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。
 ```
    <p v-if="seen">现在你看到我了</p>
```

#### 2. v-bind
绑定属性，简写 :

    <a v-bind:href="url">...</a>
一些指令能够接收一个“参数”，在指令名称之后以冒号表示，如这里的 href 是参数。
从 2.6 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数，如：

    <a v-bind:[attributeName]="url"> ... </a>
这里的 attributeName 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 data 的 property 名为 attributeName，其值为 "href"，那么这个绑定将等价于 v-bind:href。
同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：

    <a v-on:[eventName]="doSomething"> ... </a>
对动态参数的值的约束：
>动态参数预期会求出一个字符串，异常情况下值为 null。这个特殊的 null 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。
对动态参数表达式的约束：
>动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的。变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。在 DOM 中使用模板时 (直接在一个 HTML 文件里撰写模板)，还需要避免使用大写字符来命名键名，因为浏览器会把 attribute 名全部强制转为小写：

    <a v-bind:[someAttr]="value"> ... </a>
    在 DOM 中使用模板时这段代码会被转换为：v-bind:[someattr]，
    除非在实例中有一个名为“someattr”的 property，否则代码不会工作。

#### 3. v-on
>监听事件，简写 @，绑定事件时，没有传递方法参数，则方法的默认接收一个 event 参数，如果在表达式传递了参数，如"doSomething('hi')"，则它接受一个字符串参数，如果想传递事件参数，则需要使用 "doSomething('hi', $event)"

    <a v-on:click="doSomething">...</a>

修饰符 
>修饰符(modifier) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()。常见的修饰符有：.stop、.prevent、.capture、.self、.once、.passive
使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

    <form v-on:submit.prevent="onSubmit">...</form>

按键修饰符
>使用 keyCode attribute 也是允许的，Vue 提供了绝大多数常用的按键码的别名：.enter、.tab、.delete (捕获“删除”和“退格”键)、.esc、.space、.up、.down、.left、.right。还可以通过全局 config.keyCodes 对象自定义按键修饰符别名：Vue.config.keyCodes.f1 = 112。

    <input v-on:keyup.enter="submit">
    <input v-on:keyup.13="submit">

系统修饰键
>可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器:.ctrl、.alt、.shift、.meta。
.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。
```
<input v-on:keyup.alt.67="clear">  // Alt + C
<div v-on:click.ctrl="doSomething">Do something</div>  // Ctrl + Click
<button v-on:click.ctrl.exact="onCtrlClick">A</button> // 有且只有 Ctrl 被按下的时候才触发
<button v-on:click.exact="onClick">A</button>   // 没有任何系统修饰符被按下的时候才触发
```
鼠标按钮修饰符
>这些修饰符会限制处理函数仅响应特定的鼠标按钮，.left、.right、.middle

#### 4. v-for
1. 可以用 v-for 指令基于一个数组来渲染一个列表
```
    <li v-for="(item, index) in items">{{ item.message }}</li>
```
2. 可以用 of 替代 in 作为分隔符，因为它更接近 JavaScript 迭代器的语法
3. 可以用 v-for 来遍历一个对象的 property，在遍历对象时，会按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下都一致。
```
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```
4. 尽可能在使用 v-for 时提供 key attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
5. 不要使用对象或数组之类的非基本类型值作为 v-for 的 key。请用字符串或数值类型的值。
6. 在 v-for 里使用值范围
```
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```
7. 在 template 上使用 v-for，类似于 v-if，你也可以利用带有 v-for 的 template 来循环渲染一段包含多个元素的内容，比如：
```
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```
8. 在组件上使用 v-for，在自定义组件上，你可以像在任何普通元素上一样使用 v-for，此时，key 现在是必须的。然而，任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要使用 prop：
```
<my-component
    v-for="(item, index) in items"
    v-bind:item="item"
    v-bind:index="index"
    v-bind:key="item.id"
></my-component>
<li
    is="todo-item"
    v-for="(todo, index) in todos"
    v-bind:key="todo.id"
    v-bind:title="todo.title"
    v-on:remove="todos.splice(index, 1)"
></li>
注意这里的 is="todo-item" attribute，这种做法在使用 DOM 模板时是十分必要的，因为在 <ul> 元素内只有 <li> 元素会被看作有效内容。这样做实现的效果与 <todo-item> 相同，但是可以避开一些潜在的浏览器解析错误。
```

#### 5. v-model
+ 可以用 v-model 指令在表单 input、textarea 及 select 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。
+ v-model 会忽略所有表单元素的 value、checked、selected attribute 的初始值而总是将 Vue 实例的数据作为数据来源。因此应该通过 JavaScript 在组件的 data 选项中声明初始值。 
+ v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：
    + text 和 textarea 元素使用 value property 和 input 事件；
    + checkbox 和 radio 使用 checked property 和 change 事件；
    + select 字段将 value 作为 prop 并将 change 作为事件。
+ 在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步，可以添加 lazy 修饰符，从而转为在 change 事件之后进行同步，如`<input v-model.lazy="msg">` 
+ 如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符，如：`<input v-model.number="age" type="number">` 
+ 如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：`<input v-model.trim="msg">` 
+ 可以在自定义的组件上使用 v-model。