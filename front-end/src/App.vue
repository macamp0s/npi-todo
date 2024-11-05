<script setup>
import { ref, onMounted, computed } from 'vue'
import api from './services/api';
import TodoForm from './components/TodoForm.vue';

const todos = ref([])
const selectedTodo = ref(null) 
const showModal = ref(false) 
const isEditing = ref(false) 

const fetchTodos = () => {
  api.get("/api/todos")
    .then(response => {
      todos.value = response.data 
      console.log(response.data)
    })
    .catch(error => {
      console.error("Failed to fetch todos:", error)
    })
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t.id !== todo.id)
}

const detailsTodo = todo => {
  selectedTodo.value = { ...todo }
  showModal.value = true 
  isEditing.value = false 
}

const editTodo = todo => {
  selectedTodo.value = { ...todo }
  showModal.value = true 
  isEditing.value = true 
}

const closeModal = () => {
  showModal.value = false 
}

const enableEdit = () => {
  isEditing.value = true 
}

const saveEdit = () => {
  const todoIndex = todos.value.findIndex(t => t.id === selectedTodo.value.id)
  if (todoIndex !== -1) {
    todos.value[todoIndex].name = selectedTodo.value.name
    todos.value[todoIndex].content = selectedTodo.value.content
  }
  isEditing.value = false 
  showModal.value = false 
}

onMounted(() => {
  fetchTodos() 
})
</script>

<template>
  <div>
    <main class="app">
      <section class="greeting">
        <h2 class="title">TO-DO LIST</h2>
      </section>

      <TodoForm></TodoForm>

      <section class="todo-list">
        <h3>TODO LIST</h3>
        <div class="list">
          <div v-for="todo in todos" :class="`todo-item`" :key="todo.id">
            <div class="todo-content">
              <input type="text" v-model="todo.id" />
              <input type="text" v-model="todo.title" />
              <input type="text" v-model="todo.description" />
            </div>

            <div class="actions">
              <button class="edit" @click="editTodo(todo)">Edit</button>
              <button class="details" @click="detailsTodo(todo)">Details</button>
              <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
          </div>
        </div>
      </section>
    </main>

    <div v-if="showModal" class="modal-backdrop">
      <div class="modal">
        <h3><strong>Detalhes do Item</strong></h3>
        <div>
          <label><strong>Title:</strong></label>
          <div v-if="isEditing">
            <input type="text" v-model="selectedTodo.name" :disabled="false" />
          </div>
          <div v-else>
            <input type="text" v-model="selectedTodo.name" :disabled="true" />
          </div>
        </div>

        <br>
        <div>
          <label><strong>Description:</strong></label>
          <div v-if="isEditing">
            <input type="text" v-model="selectedTodo.content" :disabled="false" />
          </div>
          <div v-else>
            <input type="text" v-model="selectedTodo.content" :disabled="true" />
          </div>
        </div>
        
        <div class="modal-actions">
          <button v-if="!isEditing" @click="enableEdit">Editar</button>
          <button v-if="isEditing" @click="saveEdit">Salvar</button>
          <button @click="closeModal">Fechar</button>
        </div>
      </div>
    </div>
  </div>
</template>
