<script setup>
 
  import { ref ,watch,computed} from "vue"
  import TodoCreator from "../components/TodoCreator.vue"
  import {uid} from 'uid'
  import TodoItem from "../components/TodoItem.vue"
const todoList = ref([])

watch(todoList, () => { setTodoListLocalStorage(); }, {
  deep: true,
})

const todoComputed = computed(() => {
  return todoList.value.every((todo=>todo.isCompleted))
})

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value))
}
  const createTodo = (todo) => {
    todoList.value.push({
      id: uid(),
      todo,
      isCompleted: null,
      isEditing: false,
      
    })
  }



  

const fetchTodolist = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"))
  if (savedTodoList) {
    todoList.value = savedTodoList
  }

}


const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted
}

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing
}
const updateTodo = (todoVal, index) => {
  todoList.value[index].todo = todoVal;
}
const deleteTodo = (index) => {
  todoList.value.splice(index,1)
}


fetchTodolist();

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul>
      <TodoItem v-for="(todo,index) in todoList" 
        :todo="todo" 
        :index="index" 
        @edit-todo = "toggleEditTodo"
        @toggle-complete="toggleTodoComplete"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-if="todoList.length<=0"  class="todos-msg">😔 No todo's </p>
    <p v-else-if="todoComputed" class="todos-msg"> 🥳 you've taken care of everything ! </p>
  </main>
</template>


<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
}
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
</style>