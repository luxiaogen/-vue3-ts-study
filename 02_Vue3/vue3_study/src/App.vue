<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <Header :addTodo="addTodo" />
      <List :todos="todos" />
      <Footer
        :todos="todos"
        :checkAllTodo="checkAllTodo"
        :clearAllCompletedTodos="clearAllCompletedTodos"
      />
    </div>
  </div>
</template>

<script lang="ts">
// 引入直接的子组件
import {
  defineComponent,
  onMounted,
  provide,
  reactive,
  toRefs,
  watch,
} from 'vue'
import Header from './components/Header.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'
// 引入约束 Todo 的类型
import { Todo } from './types/Todo'
import { readTodos, saveTodos } from './utils/localStorageUtils'

export default defineComponent({
  name: 'App',
  // 注册组件
  components: { Header, List, Footer },
  // 数据应该用数组来存储，数组中的每个数据都是一个对象，对象中应该有三个属性(id,title,isCompleted)
  // 数组暂时定义在 App.vue 中
  setup() {
    const state = reactive<{ todos: Todo[] }>({
      // 开始为空
      todos: [],
    })

    // 界面加载完毕过一会再读取数据
    onMounted(() => {
      setTimeout(() => {
        state.todos = readTodos()
      }, 1000)
    })

    /* 添加一个 todo 的方法 */
    const addTodo = (todo: Todo) => {
      state.todos.unshift(todo)
    }

    /* 删除一个 todo 的方法 */
    const deleteTodo = (id: number) => {
      // state.todos.splice(id, 1)
      state.todos = state.todos.filter((todo) => todo.id != id)
    }

    /* 修改todo的isCompleted 属性的状态 */
    const updateTodo = (todo: Todo) => {
      todo.isCompleted = !todo.isCompleted
      console.log(todo)
    }
    provide('deleteTodo', deleteTodo)
    provide('updateTodo', updateTodo)

    /* 全选/全不选操作 */
    const checkAllTodo = (isCompleted: boolean) => {
      // 遍历数组
      state.todos.forEach((todo) => (todo.isCompleted = isCompleted))
    }

    /* 清除全部已完成的todo */
    const clearAllCompletedTodos = () => {
      // 要没有完成的 所以 !
      state.todos = state.todos.filter((todo) => !todo.isCompleted)
    }

    /* 检测 todos 的改变 如果改变就保存到浏览器的缓存 */
    // 对 state 里面的 内部数据 进行监视  开始深层监视
    watch(() => state.todos, saveTodos, { deep: true })

    return {
      ...toRefs(state),
      addTodo,
      deleteTodo,
      updateTodo,
      checkAllTodo,
      clearAllCompletedTodos,
    }
  },
})
</script>

<style scoped>
/*app*/
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>>
