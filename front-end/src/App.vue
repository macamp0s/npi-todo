<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const input_name = ref('')
const input_content = ref('')
const selectedTodo = ref(null) 
const showModal = ref(false) 
const isEditing = ref(false) 

const addTodo = () => {
  if (input_content.value.trim() === '' || input_name.value.trim() === '') {
    alert('Preencha os dois campos e tente novamente.')
    return
  }
  todos.value.push({
    id: Date.now(),
    name: input_name.value, 
    content: input_content.value,
    done: false,
  })
  input_name.value = ''
  input_content.value = ''
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

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

const todos_asc = computed(() =>
  todos.value.sort((a, b) => b.createdAt - a.createdAt)
)

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <div>
    <main class="app">
      <section class="greeting">
        <h2 class="title">TO-DO LIST</h2>
      </section>

      <section class="create-todo">
        <form @submit.prevent="addTodo">
          <h4> Título do Item </h4>
          <input type="text" placeholder="ex: comprar legumes" v-model="input_name" />
          <h4> Descrição do Item</h4>
          <input type="text" placeholder="ex: cenoura, batata, brócolis" v-model="input_content" />
          <input type="submit" value="Add todo" />
        </form>
      </section>

      <section class="todo-list">
        <h3>TODO LIST</h3>
        <div class="list">
          <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`" :key="todo.id">
            <div class="todo-content">
              <input type="text" v-model="todo.name" />
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="edit" @click="editTodo(todo)">Edit</button>
              <button class="details" @click="detailsTodo(todo)">Details</button>
              <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
          </div>
        </div>

        {{ todos }}
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
