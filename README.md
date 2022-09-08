# 实现
## 第一次完成
### 要有组件化编码思想
#### 1. 拆分静态组件
* 组件要按照功能点拆分，命名不要与html元素冲突
  * App
    * Header
    * List 
      * Item
    * Footer 
#### 2.实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用
* 一个组件在用：放在组件自身即可。
* 一些组件在用：放在他们共同的父组件上（<span style="color:red">状态提升</span>）。
#### 3.实现交互：从绑定事件开始
### 注意：
* props使用：
  * 父组件 ==> 子组件 通信
  * 子组件 ==> 父组件 通信（要求父先给子一个函数）
* 使用v-model时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的
* props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做
## 第二次完成
### 将APP组件与TodoHeader和TodoFooter之间的数据通信方式改成：组件自定义事件
*  组件自定义事件是一种组件间通信的方式，适用于：<strong style="color:red">子组件 ===> 父组件</strong>
*  使用场景：A是父组件，B是子组件，B想给A传数据，要在A中给B绑定自定义事件（<span style="color:red">事件的回调在A中</span>）
* 绑定自定义事件：
  * 第一种方式，在父组件中：`<Demo @event="function"/>`
  * 第二种方式，在父组件中：

        ```js
        <Demo ref="demo"/>
        ......
        mounted(){
           this.$refs.xxx.$on('atguigu',this.test)
        }
        ```
* 触发自定义事件：`this.$emit('event',data)`		
* 解绑自定义事件```this.$off(event')```
* 注意：通过`this.$refs.xxx.$on('event',回调)`绑定自定义事件时，回调<span style="color:red">要么配置在methods中</span>，<span style="color:red">要么用箭头函数</span>，否则this指向会出问题
### 将APP组件与TodoItem之间的数据通信方式改成：全局事件总线
* 适用于<span style="color:red">任意组件间通信</span>。
* 安装全局事件总线：
   ```js
   new Vue({
   	......
   	beforeCreate() {
   		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
   	},
       ......
   }) 
   ```
* 使用事件总线
  * 接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的<span style="color:red">回调留在A组件自身</span>
      ```js
      methods(){
        demo(data){......}
      }
      ......
      mounted() {
        this.$bus.$on('xxxx',this.demo)
      }
      ```
  * 提供数据：```this.$bus.$emit('xxxx',数据)```
* 最好在beforeDestroy钩子中，用$off去解绑<span style="color:red">当前组件所用到的</span>事件
## 第三次完成
### 增加todo项的编辑功能
* 点击编辑按钮时，对应todo项变成输入框，编辑按钮隐藏
  * 用`$set`方法给todo增加`isEdit`属性，默认值`true`（只在第一次增加，后续再次编辑，改变值即可）
  * `v-show`控制各项的显示隐藏，因为存在多次交互，所以用`v-show`而不是`v-if`
  * 用`nextTick`，让输入框显示时就获得焦点
    * 语法：`this.$nextTick(回调函数)`
    * 作用：在下一次 DOM 更新结束后执行其指定的回调
    * 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在`nextTick`所指定的回调函数中执行
* 输入框失去焦点是时，处理编辑逻辑
  * `isEdit=false`
  * text值不为空时，`this.$bus.$emit('updateTodo', todo.id, e.target.value)`调用函数，让APP组件改变对应id项的`todo.text`
## 第四次完成
### 给todo的添加和删除增加动画效果
* 在插入、更新或移除 DOM元素时，在合适的时候给元素添加样式类名
* 实现
  * 准备好样式：
    * 元素进入的样式
      * v-enter：进入的起点
      * v-enter-active：进入过程中
      * v-enter-to：进入的终点
    * 元素离开的样式
      * v-leave：离开的起点
      * v-leave-active：离开过程中
      * v-leave-to：离开的终点
    * 相同项可以合并写
  * 使用`<transition>`/`<transition-group>`包裹要过度的元素，并配置name属性

      ```vue
      <transition name="hello">
      	<h1 v-show="isShow">你好啊！</h1>
      </transition>
      ```
    * 若有多个元素需要过度，使用`<transition-group>`时要注意每个元素都要指定```key```值。 