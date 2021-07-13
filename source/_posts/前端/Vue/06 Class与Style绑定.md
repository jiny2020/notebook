---
title: Class 与 Style 绑定
date: 2021-05-26 12:12:57
categories: 
- 前端/Vue
tags:
- Class 与 Style 绑定
---

### 一、Class 绑定
1. class 属性绑定不会覆盖原生的样式设置，而是追加设置
2. 当做普通属性(不推荐)
3. 使用对象 对象的key是class名称，值为true/false，可以快速切换样式
    a) 对象在绑定表达式中 如：:class="{'color':class2, border: class3}"
    b) 对象在data属性中 如：:class="classObject"
4. 使用数组
5. 当在一个自定义组件上使用 class property 时，这些 class 将被添加到该组件的根元素上面

### 一、Style 绑定
1. style 属性绑定不会覆盖原生的样式设置，而是追加设置
2. 当做普通属性(不推荐)
3. 使用对象 对象的key是class名称，值为true/false，可以快速切换样式
    a) 对象在绑定表达式中 如：:class="{'color':class2, border: class3}"
    b) 对象在data属性中 如：:class="classObject"
4. 使用数组
5. 当在一个自定义组件上使用 class property 时，这些 class 将被添加到该组件的根元素上面
6. 当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS property 时，如 transform，Vue.js 会自动侦测并添加相应的前缀
7. 从 2.3.0 起你可以为 style 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：
    `<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>`
    这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。
