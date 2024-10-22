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


  
}

const todos_asc = computed(() =>todos.value.sort((a, b) =>{
  return b.createdAt - a.createdAt
}))

watch(name, (newVal)=>{
  localStorage.setItem('name', newVal)
})

onMounted(()=>{
  name.value=localStorage.getItem('name') || ''
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
      v-model="input_name"/>

    <input type="submit" value="Add todo"/>
      </form>
    </section>
   </main>
  </div>

</template>

