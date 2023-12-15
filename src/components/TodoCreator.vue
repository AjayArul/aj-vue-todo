<script setup>
import { ref, defineEmits, reactive } from "vue";
const emit = defineEmits(["create-todo"]);
const todoState = reactive({
  todo: "",
  invalid: null,
  errMsg: "",
});
const createTodo = () => {
  if (todoState.todo !== "") {
    emit("create-todo", todoState.todo);
    todoState.todo = "";
    todoState.invalid = false;
    return;
  }
  todoState.invalid = true;
  todoState.errMsg = "Todo value cannot be empty!";
};
</script>

<template>
  <div>
    <div class="input-wrap" :class="{ err: todoState.invalid }">
      <input type="text" v-model="todoState.todo" />
      <button @click="createTodo()" :disabled="todoState.todo === ''">
        Create
      </button>
    </div>
    <p v-show="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p>
  </div>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #00abf0;

  &.err {
    border: 2px solid red;
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

    &:not(:disabled) {
      background-color: #00abf0;
      color: #fff;
      cursor: pointer;
    }
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
