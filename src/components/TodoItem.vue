<template>
  <li>
    <label>
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)" />
      <!-- 如下代码也能实现功能，但是不太推荐，因为有点违反原则，因为修改了props -->
      <!-- <input type="checkbox" v-model="todo.done"/> -->
      <span>{{todo.text}}</span>
    </label>
    <button class="btn btn-danger" @click="handleTodo(todo.id)">删除</button>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  // 父组件接收数据
  props: ['todo', 'checkTodo', 'removeTodo'],
  methods: {
    // 处理勾选或取消勾选
    handleCheck(id) {
      // 通知App组件将对应的todo对象的done值取反
      this.checkTodo(id)
    },
    // 删除todo
    handleTodo(id) {
      //通知 App组件将对应的todo对象删除
      if (confirm('确定删除吗?')) {
        this.removeTodo(id)
      }
    }
  }
}
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background: #ccc;
}

li:hover button {
  display: block;
}
</style>
