---
title: Vue实例
date: 2021-05-26 12:12:57
categories: 
- 前端/Vue
tags:
- Vue实例
---

### 一、什么是Vue实例
+ 每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的
    ```
        var vm = new Vue({
            // 选项
            el:'#app',
            data:{
                msg: 'hello vue'
            }
         })
    ```
+ 一个 Vue 应用由一个通过 new Vue 创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成
+ 所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)
+ 当一个 Vue 实例被创建时，它将 data 对象中的所有的 property 加入到 Vue 的响应式系统中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值
+ 只有当实例被创建时就已经存在于 data 中的 property 才是响应式的
+ 在晚些时候需要一个 property，但是一开始它为空或不存在，那么你仅需要设置一些初始值
+ 使用 Object.freeze(obj)，这会阻止修改现有的 property，也意味着响应系统无法再追踪变化，即不能将obj作为响应式数据
+ 除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 $，以便与用户定义的 property 区分开来
    + vm.$data 用户定义的数据
    + vm.$el 挂在的DOM对象
    + vm.$watch 监听方法
        ```
            vm.$watch('msg', function (newValue, oldValue) {
                // 这个回调将在 vm.msg（data.msg） 改变后调用
            })
        ```
+ 在选项中配置方法时，通常情况下方法中的 this 就是Vue实例对象(避免使用箭头函数，它会导致 this 不再是实例对象)

### 二、生命周期
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。 
生命周期钩子的 this 上下文指向调用它的 Vue 实例。
![lifecycle](/resources/前端/Vue/lifecycle.png)

