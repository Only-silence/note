---
title: vue子父组件相互传参
date: 2020-03-26 15:30:01
tags:
- vue
- 组件
- 传参
categories:
- vue
---

##### 实现子父组件之间的相互传参<!--more-->

1. 父组件

   ```javascript
   <login :pop="login_pop" @pops="pops"/>
   //login是子组件
   //子组件绑定pop变量
   //pop是父组件传递内容  @pops是自定义事件
   ```

   ```javascript
   pops(e){
         this.login_pop = e //接收子组件传递的参数
       }
   ```

   

2. 子组件

   ```javascript
   export default {
       data(){
           return {
           }
       },
       props:['pop'],  //接收父组件传递的参数
       methods:{
           close() {
               this.pop = !this.pop  //使用传递过来的参数
               this.$emit('pops',this.pop)
               //通过绑定事件和$emit传值
               //将值绑定到pops上传递过去
           }
       }
   }
   ```