<script setup>
import { ref } from "vue";
import { uid } from "uid";
import { Icon } from "@iconify/vue";
import TodoCreator from "../components/TodoCreator.vue";
import TodoItem from "../components/TodoItem.vue";

const todoList = ref([]);
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: null,
  });
};

const getIndexWithId = (list, id) => {
  return list?.findIndex((obj) => obj.id === id);
};

const toggleTodoComplete = (id) => {
  const index = getIndexWithId(todoList?.value, id);
  index !== -1 &&
    (todoList.value[index].isCompleted = !todoList.value[index].isCompleted);
};

const toggleEditTodo = (id) => {
  const index = getIndexWithId(todoList?.value, id);
  index !== -1 &&
    (todoList.value[index].isEditing = !todoList.value[index].isEditing);
};

const updateTodo = (value, id) => {
  const index = getIndexWithId(todoList?.value, id);
  index !== -1 && (todoList.value[index].todo = value);
};

const deleteTodo = (id) => {
  todoList.value = todoList.value.filter((value) => value.id !== id);
};
</script>

<template>
  <main>
    <h1>Create Todos</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="todo in todoList"
        :todo="todo"
        :key="todo.id"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
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
}
</style>
