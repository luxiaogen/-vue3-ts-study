<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { Todo } from '../types/Todo'

export default defineComponent({
  name: 'Header',
  props: {
    // 接收 App 传过来的方法
    addTodo: {
      type: Function,
      required: true, // 必须的
    },
  },
  setup(props) {
    // 定义一个 ref 数据
    const title = ref('')

    // 添加 todo 的方法
    const add = () => {
      // 获取文本框的内容
      const text = title.value
      // 判断 text 是否为空
      // eslint-disable-next-line no-undef
      if (!text.trim()) return alert('添加不能为空')
      // 说明 文本框 不为空
      const todo: Todo = {
        id: Date.now(),
        title: text,
        isCompleted: false,
      }
      // 调用父组件传递过来的数据
      props.addTodo(todo)
      // 情况文本框
      title.value = ''
    }

    return {
      add,
      title,
    }
  },
})
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
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>>
