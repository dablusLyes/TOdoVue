
<script setup>
    import { reactive, ref, defineEmits } from "vue"
    import TodoButton from "./TodoButton.vue"
    let todo = ref("")

    const todoState = reactive({
            todo: "",
            invalid: null,
            errMsg:"",
        })

    const emit = defineEmits(['create-todo'])
    const createTodo = () => {
        todoState.invalid = null;
        if (todoState.todo !== "") {
            emit('create-todo',todoState.todo)
            todoState.todo = ""
            return;
        }

        todoState.invalid = true;
        todoState.errMsg = "Todo value cannot be empty"
    }


</script>


<template>
    <div>
        <div class="input-wrap"  @keyup.enter="createTodo()" :class="{ 'inputErr' : todoState.invalid }">
            <input type="text" v-model="todoState.todo">
            <TodoButton @click="createTodo()" > ➡️ </TodoButton>
        </div>
    </div>
    <p  v-show="todoState.invalid" class="errMsg">{{ todoState.errMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}
.errMsg{
    color: red;
    text-align: center;
}
.inputErr{
    border: 2px solid rgb(184, 77, 77);
}

</style>