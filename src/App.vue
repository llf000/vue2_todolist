<template>
  <div id="root">
    <HelloWorld msg="Vue2 TodoList" />
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader :addTodo="addTodo" />
        <TodoList :todos="todos" :checkTodo="checkTodo" :removeTodo="removeTodo" />
        <TodoFooter :todos="todos" :checkAllTodo='checkAllTodo' :clearAllTodo="clearAllTodo" />
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/Hello.vue'

import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    TodoHeader,
    TodoList,
    TodoFooter
  },
  data() {
    return {
      // 将todos写在app中是因为Header（写）和Footer（读）、List（读）都要使用todos，这属于 状态提升
      todos: [] || JSON.parse(localStorage.getItem('todos'))
    }
  },
  methods: {
    // 添加新todo
    addTodo(todo) {
      this.todos.push(todo)
    },
    // 移除todo
    removeTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
    },
    // 勾选或取消勾选todo--每个todo项前的复选框
    checkTodo(id) {
      this.todos.forEach(todo => {
        if (todo.id === id) {
          todo.done = !todo.done
        }
      })
    },
    // 全选或者取消全选--Footer前的复选框
    checkAllTodo(done) {
      this.todos.forEach(todo => todo.done = done)
    },
    // 清除所有已完成的todo项
    clearAllTodo() {
      this.todos = this.todos.filter(todo => !todo.done)
    }
  },
  watch: {
    // 需要深度监视
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem('todos', JSON.stringify(value))
      }
    }
  }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
