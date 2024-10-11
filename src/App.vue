```vue
<script setup>
import { reactive, ref, computed } from 'vue';
const tasks = reactive([
  { id: 1, description: 'Estudar ES6', completed: false },
  { id: 2, description: 'Estudar SASS', completed: false },
  { id: 3, description: 'Ir trabalhar', completed: true },
]);

const newTaskDescription = ref('');
const filter = ref('todas');

function addTask() {
  if (newTaskDescription.value.trim()) {
    const newTask = {
      id: tasks.length + 1,
      description: newTaskDescription.value,
      completed: false,
    };
    tasks.push(newTask);
    newTaskDescription.value = '';
  }
}

const filteredTasks = computed(() => {
  switch (filter.value) {
    case 'pendentes':
      return tasks.filter(task => !task.completed);
    case 'finalizadas':
      return tasks.filter(task => task.completed);
    default:
      return tasks;
  }
});

const pendingTasksCount = computed(() => tasks.filter(task => !task.completed).length); //Verifica as tarefas pendentes
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ pendingTasksCount }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="addTask" class="row">
      <div class="col">
        <input required v-model="newTaskDescription" type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select v-model="filter" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="task in filteredTasks" :key="task.id">
        <input v-model="task.completed" type="checkbox" :id="task.id">
        <label :class="{ done: task.completed }" class="ms-3" :for="task.id">
          {{ task.description }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>