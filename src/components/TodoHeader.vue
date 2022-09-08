<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="newTodoText" @keyup.enter="add" />
  </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
  name: 'TodoHeader',
  // 接收数据
  props: ['addTodo'],
  data() {
    return {
      // 收集输入的text
      newTodoText: ''
    }
  },
  methods: {
    add() {
      // 校验数据--去掉前后空格
      const trimmedtext = this.newTodoText.trim()
      if (trimmedtext) {
        // 将输入的信息包装成todo对象
        const todo = { id: nanoid(), text: trimmedtext, done: false }
        // 通知App组件添加新todo
        this.addTodo(todo)
        // 清空输入
        this.newTodoText = ''
      } else {
        alert('输入不能为空')
      }
    }
  }
}
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>
