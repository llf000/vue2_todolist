<template>
  <li>
    <label>
      <!-- 如下代码也能实现功能，但是不太推荐，因为有点违反原则，因为修改了props -->
      <!-- <input type="checkbox" v-model="todo.done"/> -->
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)" />

      <!-- 编辑模式时，文字变成输入框，可以更改todo.text -->
      <input type="text" v-show="todo.isEdit" :value="todo.text" @blur="handleBlur(todo,$event)" ref="inputText" />
      <span v-show="!todo.isEdit">{{todo.text}}</span>
    </label>
    <button class="btn btn-danger" @click="handleTodo(todo.id)">删除</button>
    <button class="btn btn-edit" v-show="!todo.isEdit" @click="handleEdit(todo)">编辑</button>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  // 父组件接收数据
  props: ['todo'],
  methods: {
    // 处理勾选或取消勾选
    handleCheck(id) {
      // 通知App组件将对应的todo对象的done值取反
      // 调用自定义事件checktodo，并传递参数id
      this.$bus.$emit('checkTodo', id)
    },
    // 删除todo
    handleTodo(id) {
      // 调用$bus全局事件总线 通知 App组件将对应的todo对象删除
      if (confirm('确定删除吗?')) {
        this.$bus.$emit('removeTodo', id)
      }
    },
    // 编辑
    handleEdit(todo) {
      // todo对象第一次编辑时，会增加值为真的isEdit属性，以后再次调用编辑时只需要将isEdit值改为真就可
      if (Object.prototype.hasOwnProperty.call(todo, 'isEdit')) {
        todo.isEdit = true
      } else {
        this.$set(todo, 'isEdit', true)
      }
      // 在模板解析完成后调用回调函数让输入框获得焦点
      this.$nextTick(function () {
        this.$refs.inputText.focus()
      })
    },
    // 失去焦点回调 真正处理编辑逻辑
    handleBlur(todo, e) {
      todo.isEdit = false
      if (e.target.value.trim()) {
        this.$bus.$emit('updateTodo', todo.id, e.target.value)
      } else {
        alert('输入不能为空')
      }
    }
  },
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
