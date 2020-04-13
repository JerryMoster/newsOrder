<template>
  <div class="todo-contaiiner">
    <h2>消息的订阅与发布【这里改造的是 Item】 </h2>
    <div class="todo-wrap">

      <Head ref="head" />
      <List :todoL="todos" :deleteTodo="deleteTodo" />
      <Foot :todosfoot="todos" :selectedAll="selectedAll" :deltetAll="deltetAll" />
    </div>
  </div>
</template>

<script>
import Head from './components/Head.vue'
import List from './components/List.vue'
import Foot from './components/Foot.vue'

// 引入工具类
import localStorageUtils from './utils/localStorageUtils.js'
// 引入消息发布与订阅这种方式传递数据的第三方插件 pubsub.js，这个插件，谁发布，在谁里面引用，谁触发，谁订阅，就是谁用，这里是 Item 用
import PubSub from 'pubsub-js'

export default {
  name: 'App',
  data () {
    return {
      todos: localStorageUtils.readTodos()
    }
  },
  components: {
    Head,
    List,
    Foot
  },
  mounted () {
    // 绑定自定义事件的监听
    this.$refs.head.$on('addTodo', this.addTodo)
    // 订阅消息（deleteTodo）
    PubSub.subscribe('deleteTodo', (msg, token) => {
      console.log(msg, token)
      this.deleteTodo(token)
    })
  },
  methods: {
    // 根据索引删除这条纪律
    deleteTodo (index) {
      this.todos.splice(index, 1)
    },
    // 插入一条新的数据
    addTodo (todo) {
      this.todos.unshift(todo)
    },
    selectedAll (isCheck) {
      this.todos.forEach(function (todo) {
        todo.finished = isCheck
      })
    },
    // 删除选中的项目
    deltetAll () {
      this.todos = this.todos.filter(todo => !todo.finished)
    }
  },
  watch: {
    // 深度监事
    todos: {
      handler: localStorageUtils.saveTodos,
      deep: true, // 深层次的监视
      immediate: true // 已经来就触发回调函数 handler
    }
  }
}
</script>

<style scoped>
h2 {
  text-align: center;
}
</style>
