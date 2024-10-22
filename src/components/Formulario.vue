<script setup>
const props = defineProps({
    descricaoNovaTarefa: {
        type: String,
        required: true
    },
    filter: {
        type: String,
        required: true
    }
});

const emit = defineEmits(['adicionaTarefa', 'update:descricaoNovaTarefa', 'update:filter']);
</script>

<template>
    <form @submit.prevent="emit('adicionaTarefa')" class="row">
        <div class="col">
            <input required :value="descricaoNovaTarefa" @input="emit('update:descricaoNovaTarefa', $event.target.value)"
                type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control" />
        </div>
        <div class="col-md-2">
            <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
            <select :value="filter" @change="emit('update:filter', $event.target.value)" class="form-control">
                <option value="todas">Todas tarefas</option>
                <option value="pendentes">Pendentes</option>
                <option value="finalizadas">Finalizadas</option>
            </select>
        </div>
    </form>
</template>
