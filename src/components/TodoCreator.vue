
<script setup>
    import { reactive, ref, defineEmits } from "vue"
    import TodoButton from "./TodoButton.vue"
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
        todoState.errMsg = "Todo Can't be empty"
    }
</script>


<template>
    <div>
        <div class="input-wrap"  @keyup.enter="createTodo()" :class="{ 'inputErr' : todoState.invalid }">
            <input type="text" v-model="todoState.todo">
            <TodoButton @click="createTodo()" > Create  </TodoButton>
        </div>
    </div>
    <p  v-show="todoState.invalid" class="errMsg">{{ todoState.errMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

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

  button {
    padding: 8px 16px;
    border: none;
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>