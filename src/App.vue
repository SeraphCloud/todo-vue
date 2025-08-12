<script setup>
import { reactive } from 'vue'
import Header from './components/Header.vue'
import Form from './components/Form.vue'
import Tasklist from './components/Tasklist.vue'

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
    <Header :tarefas-pendentes="getTarefasPendentes().length" />
    <Form
      :trocar-filtro="(e) => (state.filtro = e.target.value)"
      :tarefa-temp="state.tarefaTemp"
      :edita-tarefa-temp="(e) => (state.tarefaTemp = e.target.value)"
      :cadastra-tarefa="cadastraTarefa"
    />
    <tasklist :tarefas="getTarefasFiltradas()" :filtro="state.filtro" />
  </div>
</template>
