<script setup>
import HelloWorld from './components/HelloWorld.vue'
import {ref, onMounted, computed, watch} from 'vue'

const todos = ref([])
const id = ref('')
const name = ref('')
const content = ref('')

const input_name = ref ('')
const input_content = ref ('')

const addTodo = () => {
if (input_content.value.trim()===''||input_name.value.trim()===''){
  return
}
todos.value.push({
  name:input_name.value,
  content:input_content.value,
  done:false,
  createdAt:new Date().getTime()
})
}

const removeTodo = todo =>{
  todos.value = todos.value.filter(t=> t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep:true})

const todos_asc = computed(() =>todos.value.sort((a, b) =>{
  return b.createdAt - a.createdAt
}))

watch(name, (newVal)=>{
  localStorage.setItem('name', newVal)
})

onMounted(()=>{
  name.value=localStorage.getItem('name') || ''
  todos.value=JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
  <div>
   <main class="app">
    <section class="greeting">
      <h2 class="title">
        Test <input type="text" placeholder="Insira o nome" v-model="name">
      </h2>
    </section>

    <section class="create-todo">
      <h3>CRIAR TODO</h3>
      <form @submit.prevent="addTodo">
      <h4> Título do Item </h4>
      <input 
      type="text" 
      placeholder="ex: comprar legumes"
      v-model="input_name"/>

      <h4> Descrição do Item</h4>
      <input 
      type="text" 
      placeholder="ex: cenoura, batata, brócolis"
      v-model="input_content"/>

    <input type="submit" value="Add todo"/>
      </form>
    </section>
    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && `done`}`">
          

          <div class="todo-content">
            <input type="text" v-model="todo.name" />
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>          
          </div>
        </div>
      </div>


    </section>
 
   </main>
  </div>

</template>

