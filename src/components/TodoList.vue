<template>
  <ul class="todo-main">
    <transition-group name="todoAnimate" appear>
      <TodoItem v-for="todo in todos " :key="todo.id" :todo="todo" />
    </transition-group>

  </ul>

</template>

<script>
import TodoItem from './TodoItem.vue'

export default {
  name: 'TodoList',
  components: { TodoItem },
  // 接收App传过来的数据
  props: ['todos']
}

</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

/* 进入的起点 离开的终点 */
.todoAnimate-enter,
.todoAnimate-leave-to {
  transform: translate(-100%)
}

/* 进入和离开的动画持续0.5s 匀速 */
.todoAnimate-enter-active,
.todoAnimate-leave-active {
  transition: 0.5s linear;
}

/* 进入的终点 离开的起点 */
.todoAnimate-enter-to,
.todoAnimate-leave {
  transform: translate(0%)
}
</style>
