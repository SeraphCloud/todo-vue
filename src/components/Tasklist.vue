<script setup>
const props = defineProps(['tarefas', 'filtro'])
</script>

<template>
  <ul v-if="props.tarefas.length > 0" class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in props.tarefas" :key="tarefa.titulo">
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
  <p v-else class="mt-4">
    {{
      props.filtro === 'pendentes'
        ? 'Não existem mais tarefas pendentes'
        : props.filtro === 'finalizadas'
          ? 'Não existem tarefas finalizadas'
          : 'Não existem tarefas cadastradas'
    }}
  </p>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
