<template>
  <div id="root">
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <div class="todo-container">
      <div class="todo-wrap">
        <!-- <TodoHeader :addTodo="addTodo" /> -->
        <!-- 给header添加自定义事件 @事件名：回调函数 -->
        <TodoHeader @addTodo="addTodo" />
        <TodoList :todos="todos" />
        <TodoFooter :todos="todos" @checkAllTodo='checkAllTodo' @clearAllTodo="clearAllTodo" />
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

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
    // 编辑todo
    updateTodo(id, text) {
      this.todos.forEach(todo => { if (todo.id === id) todo.text = text })
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
  },
  // 在组件挂载后就找到全局时间总线，绑定事件，在事件被触发时会调用对应回调函数
  // 实现APP和item之间的通信---APP需要在todo移除和复选框操作时，接收item的数据
  mounted() {
    this.$bus.$on('checkTodo', this.checkTodo)
    this.$bus.$on('removeTodo', this.removeTodo)
    this.$bus.$on('updateTodo', this.updateTodo)
  },
  // 在组件被销毁前需要取消自定义动的事件
  beforeDestroy() {
    this.$bus.$off('checkTodo')
    this.$bus.$off('removeTodo')
    this.$bus.$off('updateTodo')
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

.btn-edit {
  color: #fff;
  background-color: #494bda;
  border: 1px solid #4b2fbd;
}

.btn-edit:hover {
  color: #fff;
  background-color: #4b2fbd;
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
