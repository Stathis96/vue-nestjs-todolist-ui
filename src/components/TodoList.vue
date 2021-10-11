<template>
  <div>
    <el-row>
      <el-col :span="12" :offset="7" style="width:100%">
       <h1>{{ msg }}</h1>
       <TodoForm @send-message="createTodo"/>
       <el-table :data="todos">
         <el-table-column prop="title" label="Title"></el-table-column>
       </el-table>
      </el-col>
    </el-row>
  </div>
</template>

<script lang="ts">
import { ElMessage } from "element-plus";
import { defineComponent, onMounted, ref} from "vue";
import TodoForm from './TodoForm.vue'
import axios from 'axios'

export default defineComponent({
  name: 'TodoList',
  components:{ TodoForm },
   props: {
    msg: String
  },
  setup() {
    let todos = ref<[]>([])

    onMounted(async() => {
      await loadTodos()
      console.log("onmounted called and i got ", await loadTodos())
    })

    const loadTodos = async() => {
      const response = await axios.get('http://localhost:8000/todos')
      todos.value = response.data
      console.log("what did it fetch",todos)
    }

     const createTodo = async (todo:any) =>{
      console.log("todo", todo)
      ElMessage({
        message: "Todo Created",
        type: "success"
      })
    }
    return {
      createTodo,
      loadTodos,
      todos
    }
  }
})
</script>

<style lang="scss" scoped>

</style>