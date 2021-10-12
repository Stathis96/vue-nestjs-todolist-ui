<template>
  <div>
    <el-row>
      <el-col :span="12" :offset="7" style="width:100%">
       <h1>{{ msg }}</h1>
       <TodoForm @send-message="createTodo"/>
       <el-table :data="todos">
          <el-table-column prop="title" label="Title"></el-table-column>
          <el-table-column label="Operations" fixed="right" width="200">
            <template #default="scope">
              <el-space wrap>
                <el-switch v-model="scope.row.completed" @click="updateTodo(scope.row)">
                </el-switch>
                <el-popconfirm 
                  confirm-button-text="Yes"
                  cancel-button-text="No"
                  icon="el-icon-info"
                  icon-color="red"
                  title="Are you sure you want to delete this?"
                  @confirm="deleteTodo(scope.row)"
                  @cancel="cancelDelete()"
                >
                <template #reference>
                  <el-button size="mini" type="danger">
                    Delete
                  </el-button>
                </template>
                </el-popconfirm>
              </el-space>
            </template>
          </el-table-column>
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

interface Todo{
  id: number,
  title: string,
  completed: boolean
}

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
      // console.log("onmounted called and i got ", await loadTodos())
    })

    const loadTodos = async() => {
      const response = await axios.get('http://localhost:8000/todos')
      todos.value = response.data
      console.log("what did it fetch",todos)
    }

     const createTodo = async (todo: Todo) =>{
      console.log("todo", todo)
      await axios.post(`http://localhost:8000/todos`,{
        title: todo.title,
        completed: todo.completed
      })
      ElMessage({
        message: "Todo Created",
        type: "success"
      })
      await loadTodos()
    }

      const updateTodo = async (todo: Todo) =>{
        console.log("todo", todo)
        await axios.put(`http://localhost:8000/todos/${todo.id}`,{
          id : todo.id,
          title: todo.title,
          completed: todo.completed
        })
        ElMessage({
          message: "Todo Updated",
          type: "success"
        })
        await loadTodos()
     }

      const deleteTodo = async (todo: Todo) =>{
        console.log("todo", todo)
        await axios.delete(`http://localhost:8000/todos/${todo.id}`)
        ElMessage({
          message: "Todo Deleted",
          type: "success"
        })
        await loadTodos()
      }
      const cancelDelete = () =>{
        console.log("Cancelled deletion")
      }     
    return {
      createTodo,
      updateTodo,
      deleteTodo,
      loadTodos,
      todos,
      cancelDelete
    }
  }
})
</script>

<style lang="scss" scoped>

</style>