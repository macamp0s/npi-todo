<script setup>
    import { ref} from 'vue'

    const input_name = ref('')
    const input_content = ref('') 

    const addTodo = async () => {
  try {
    const res = api.post("/api/todos", {
      id: Date.now(),
      title: input_name.value,
      description: input_content.value,
      completed: false
    })
    todos.value.push(res.data)
    input_name.value = ''
    input_content.value = ''
  } catch (e) {
    console.log(e)
    } 
  }

    </script>
    
    <template>

        <section class="create-todo">
            <form @submit.prevent="addTodo">
              <h4> Título do Item </h4>
              <input type="text" placeholder="ex: comprar legumes" v-model="input_name" />
              <h4> Descrição do Item</h4>
              <input type="text" placeholder="ex: cenoura, batata, brócolis" v-model="input_content" />
              <input type="submit" value="Add todo" />
            </form>
          </section>

    </template>
