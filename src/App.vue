<template>
<el-container>
    <el-main>
      <TodoList msg="Another to-do-list made with Vue and Nest"/>
    </el-main>
  </el-container>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import TodoList from './components/TodoList.vue';

@Options({
  components: {
    TodoList,
  },
})
export default class App extends Vue {}
</script>

<style>
body {
  margin: 0;
}
.el-container{
  background-color: #e9eef3;
  color: var(--el-tet-color-primary);
  text-align: center;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

}
</style>
