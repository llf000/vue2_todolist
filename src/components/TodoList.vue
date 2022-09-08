<template>
  <ul class="todo-main">
    <TodoItem v-for="todo in todos " :key="todo.id" :todo="todo" />
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
</style>
