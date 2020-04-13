<template>
  <div>
    <li @mouseenter="dealShow(true)" @mouseleave="dealShow(false)" :style="{background:bgColor}">
      <label for="">
        <input type="checkbox" v-model="todo.finished" />
        <span>{{todo.title}}</span>
      </label>
      <button v-show="isShowDelButton" class="btn btn-waring" @click="DeleteItem()">删除</button>
    </li>
  </div>
</template>

<script>
// 引入消息发布与订阅这种方式传递数据的第三方插件 pubsub.js，这个插件，谁发布，在谁里面引用，谁触发，谁订阅，就是谁用，这里是 Item 用
import PubSub from 'pubsub-js'

export default {
  props: {
    todo: Object,
    index: Number,
    deleteT: Function
  },
  data () {
    return {
      isShowDelButton: false,
      bgColor: '#fff'
    }
  },
  computed: {

  },
  created () {

  },
  mounted () {

  },
  watch: {

  },
  methods: {
    // 控制鼠标移入和移出，删除按钮的显示隐藏
    dealShow (isShow) {
      this.isShowDelButton = isShow
      // 控制背景颜色
      this.bgColor = isShow ? '#ccc' : '#fff'
    },
    // 删除事件
    DeleteItem () {
      if (window.confirm(`确定删除${this.todo.title}吗？`)) {
        // this.deleteT(this.index)
        PubSub.publish('deleteTodo', this.index)
      }
    }
  },
  components: {

  }
}
</script>

<style lang="scss" scoped>
li {
  width: 100%;
  height: 40px;
  line-height: 40px;
  text-align: left;
  border: 1px solid #ccc;
}

label {
  margin-right: 10px;
}

button {
  float: right;
  width: 60px;
  height: 30px;
  margin-top: 5px;
  margin-right: 10px;
  border: none;
}
</style>
