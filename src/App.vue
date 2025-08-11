<script setup>
import { reactive } from 'vue'

const state = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    },
  ],
})

const getTarefasPendentes = () => {
  return state.tarefas.filter((tarefa) => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
  return state.tarefas.filter((tarefa) => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtro } = state

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes()
    case 'finalizadas':
      return getTarefasFinalizadas()
    default:
      return state.tarefas
  }
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: state.tarefaTemp,
    finalizada: false,
  }
  state.tarefas.push(novaTarefa)
  state.tarefaTemp = ''
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-5 bg-light rounded-3">
      <h1>Lista de tarefas</h1>
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input
            :value="state.tarefaTemp"
            @change="(e) => (state.tarefaTemp = e.target.value)"
            required
            class="form-control"
            type="text"
            placeholder="Digite aqui a tarefa"
          />
        </div>
        <div class="col-md-2">
          <button class="btn btn-primary" type="submit">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="(e) => (state.filtro = e.target.value)" class="form-control">
            <option value="todas">Todas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()" :key="tarefa.titulo">
        <input
          @change="(e) => (tarefa.finalizada = e.target.checked)"
          :checked="tarefa.finalizada"
          :id="tarefa.titulo"
          type="checkbox"
        />
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">{{
          tarefa.titulo
        }}</label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
