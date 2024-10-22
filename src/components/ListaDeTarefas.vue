<script setup>
const props = defineProps({
    filtraTarefas: {
        type: Array,
        required: true
    }
});

const emit = defineEmits(['updateTask']);
</script>

<template>
    <div>
        <ul class="list-group mt-4" v-if="filtraTarefas.length > 0">
            <li class="list-group-item" v-for="task in filtraTarefas" :key="task.id">
                <input :checked="task.completed" type="checkbox" :id="task.id"
                    @change="emit('updateTask', { id: task.id, completed: !task.completed })" />
                <label :class="{ done: task.completed }" class="ms-3" :for="task.id">
                    {{ task.description }}
                </label>
            </li>
        </ul>
        <p v-else>Não existem tarefas pendentes</p>
    </div>
</template>

<style scoped>
.done {
    text-decoration: line-through;
}
p {
    text-align: center;
    font-weight: bold;
    margin-top: 5em;
}
</style>
