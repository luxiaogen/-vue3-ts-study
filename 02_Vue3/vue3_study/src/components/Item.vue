<!-- eslint-disable vue/no-mutating-props -->
<template>
  <li
    @mouseenter="mouseHandle(true)"
    @mouseleave="mouseHandle(false)"
    :style="{backgroundColor:bgColor,color:myColor}"
  >
    <label>
      <input type="checkbox" v-model="isCompleted" />
      <span>{{ todo.title }}</span>
    </label>
    <button class="btn btn-danger" v-show="isShow" @click="HandleDelete(todo.id)">删除</button>
  </li>
</template>

<script lang="ts">
import { computed, defineComponent, inject, ref } from 'vue'
import { Todo } from '../types/Todo'

export default defineComponent({
  name: 'Item',
  props: {
    // todo: Object as () => Todo,
    todo: {
      type: Object as () => Todo,
      required: true,
    },
  },
  setup(props) {
    // 背景色
    const bgColor = ref('white')
    // 字体颜色
    const myColor = ref('black')
    // 控制删除按钮是否显示
    const isShow = ref(false)
    // 鼠标进入和离开的事件
    const mouseHandle = (flag: boolean) => {
      if (flag) {
        bgColor.value = 'skyblue'
        myColor.value = 'green'
        isShow.value = true
      } else {
        bgColor.value = 'white'
        myColor.value = 'black'
        isShow.value = false
      }
    }

    // 接收 App 删除的方法
    const deleteTodo = <Function>inject('deleteTodo')

    // 执行删除的方法
    const HandleDelete = (id: number) => {
      if (window.confirm('确认要删除吗')) {
        deleteTodo(id)
      }
    }

    // 计算属性方式 --- 来让当前复选框选中/选不中
    const updateTodo = <Function>inject('updateTodo')
    const isCompleted = computed({
      get() {
        return props.todo.isCompleted
      },
      set() {
        updateTodo(props.todo)
      },
    })

    return {
      mouseHandle,
      bgColor,
      myColor,
      isShow,
      HandleDelete,
      isCompleted,
    }
  },
})
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
  /* display: none; */
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>>
