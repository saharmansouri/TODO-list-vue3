<script setup lang="ts">
import { onBeforeMount, reactive, ref } from "vue";

interface TypeTodo {
  text: string | number;
  completed: boolean;
  id: number | string;
}

const todos = ref<TypeTodo[]>([
  { text: "Pay bills", completed: false, id: 1 },
  { text: "Meet George", completed: false, id: 2 },
  { text: "Buy eggs", completed: false, id: 3 },
  { text: "Read a book", completed: false, id: 4 },
  { text: "Organize office", completed: false, id: 5 },
]);

const input = ref("");
const editInput = ref<TypeTodo>({ id: "", completed: false, text: "" });
const showModal = ref(false);
// onBeforeMount(() => {
//   const cachedTodos = localStorage.getItem("todos");
//   if (cachedTodos) {
//     todos.value = JSON.parse(cachedTodos);
//   }
// });  

const addTodo = () => {
  if (input.value.trim()) {
    todos.value.push({
      text: input.value,
      completed: false,
      id: todos.value.length + 1,
    });
    console.log("input.value:",input.value)
    input.value = "";
    // UpdateStorage();
  }
};

function EditTodo(todoItem: TypeTodo) {
  const EditedTodo = todos.value.find((todo) => todo.id === todoItem.id);
  if(EditedTodo !==undefined){
    EditedTodo.text = todoItem.text;
    todoItem.text = "";
    console.log("edit:",EditedTodo.text)
    UpdateStorage();
  }
}
function tryToEditItem(todoId: string) {
  editInput.value.id= todoId;
}

const completedTodo = (id: string | number) => {
  todos.value.map((todo) => {
    if (todo.id === id) {
      todo.completed = !todo.completed;
    }
  });
  UpdateStorage();
};

function deleteTodo(todoItem: TypeTodo) {
  const deletedTodo = todos.value.findIndex((todo) => todo === todoItem);
  todos.value.splice(deletedTodo, 1);
  UpdateStorage()
}

function UpdateStorage() {
  localStorage.setItem("todos", JSON.stringify(todos.value));
}
</script>

<template>
      <form @submit="addTodo()">
  <div class="header">
    <h2 style="margin: 5px">My To Do List</h2>
    <input type="text" placeholder="Title..." v-model="input" />
    <span class="addBtn" @click.="addTodo">Add</span>
  </div>
</form >


  <ul>
    <li
    v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
      
      <span @click="deleteTodo(todo)"  class="close">×</span>
      <span @click="completedTodo(todo.id)"
       class="complte"
       :class="{checked:todo.completed}"
        >completed</span>
      <span class="Edit"
      @click="tryToEditItem(todoId)
          showModal = true;
          editInput.id = todo.id;">
      Edit
    </span>
    
    <div v-if="showModal" class="modal">
      <input
      type="text"
      v-model="editInput.text"
      placeholder="Edit your task"
      class="inputEdit"
      />
      <button
      @click="
            showModal = false;
            EditTodo(editInput);
            "
          class="close Editclose"
          >
          Done
        </button>
      </div>
    </li>
  </ul>
</template>

<style scoped>
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  width: 300px;
  margin-left: -150px;
  z-index: 999;
}
input.inputEdit {
  background-color: #d9d9d9;
  width: 100%;
}
.Editclose {
  padding: 9px 12px 10px 20px;
}
</style>



 