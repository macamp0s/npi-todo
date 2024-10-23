<div v-if="showModal" class="modal-backdrop">
    <div class="modal">
      <h3><strong>Detalhes do Item</strong></h3>
      <div>
        <label><strong>Title:</strong></label>
        <div v-if="isEditing">
          <input type="text" v-model="selectedTodo.name" :disabled='false' />
        </div>
        <div v-else>
          <input type="text" v-model="selectedTodo.content" :disabled='true'/>
        </div>
      </div>

      <br>
      <div>
        <label><strong>Description:</strong></label>
        <div v-if="isEditing">
          <input type="text" v-model="selectedTodo.content" :disabled='false'/>
        </div>
        <div v-else>
          <input type="text" v-model="selectedTodo.content" :disabled='true'/>
        </div>
      </div>
      

      <div class="modal-actions">
        <button v-if="!isEditing" @click="enableEdit">Editar</button>
        <button v-if="isEditing" @click="saveEdit" >Salvar</button>
        <button @click="closeModal">Fechar</button>
      </div>
    </div>
  </div>