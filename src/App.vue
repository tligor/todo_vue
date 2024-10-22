<script setup>
import { reactive, ref, computed } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

const tarefas = reactive([
  { id: 1, description: 'Estudar ES6', completed: false },
  { id: 2, description: 'Estudar SASS', completed: false },
  { id: 3, description: 'Ir trabalhar', completed: true },
]);

const descricaoNovaTarefa = ref('');
const filter = ref('todas');

function adicionaTarefa() {
  if (descricaoNovaTarefa.value.trim()) {
    tarefas.push({
      id: tarefas.length + 1,
      description: descricaoNovaTarefa.value,
      completed: false,
    });
    descricaoNovaTarefa.value = '';
  }
}

const filtraTarefas = computed(() => {
  return filter.value === 'pendentes' 
    ?tarefas.filter(task => !task.completed)
    :filter.value === 'finalizadas'
    ?tarefas.filter(task => task.completed)
    :tarefas;
});

const tarefasPendentes = computed(() => tarefas.filter(task => !task.completed).length);
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="tarefasPendentes" />
    <Formulario
      :descricaoNovaTarefa="descricaoNovaTarefa"
      :filter="filter"
      @adicionaTarefa="adicionaTarefa"
      @update:descricaoNovaTarefa="descricaoNovaTarefa = $event"
      @update:filter="filter = $event"
    />
    <ListaDeTarefas
      :filtraTarefas="filtraTarefas"
      @updateTask="task => (tarefas.find(t => t.id === task.id).completed = task.completed)"
    />
  </div>
</template>

<style scoped>
</style>
