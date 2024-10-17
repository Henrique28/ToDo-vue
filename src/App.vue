<script setup>
import { reactive } from 'vue';


const estado = reactive({
    filtro: 'todas',
    tarefaTem: '',
    tarefas:[]
});



const cadastraTarefa = () => {
    const tarefaNova = {
        titulo: estado.tarefaTem,
        finalizada: false
    }

    estado.tarefas.push(tarefaNova)
    estado.tarefaTem = '';
}

const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
    const {filtro} = estado;

    switch(filtro){
        case 'pendentes':
            return getTarefasPendentes();
        case 'finalizadas':
            return getTarefasFinalizadas();
        default:
            return estado.tarefas
    }
}
</script>

<template>
    <div class="container">
        <header class="p-5 my-4 bg-light rounded-3">
            <h1>Minhas Tarefas</h1>
            <p>
                Você possui {{ getTarefasPendentes().length }} tarefas pendentes
            </p>
        </header>

        <form @submit.prevent="cadastraTarefa">
            <div class="row">
                <div class="col">
                    <input :value="estado.tarefaTem" @change="evento => estado.tarefaTem = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
                </div>
                <div class="col-md-2">
                    <button type="submit" class="btn btn-primary">Cadastrar</button>
                </div>
                <div class="col-md-2">
                    <select @change="evento => estado.filtro = evento.target.value" class="form-control">
                        <option value="todas">Todas tarefas</option>
                        <option value="pendentes">Pendentes</option>
                        <option value="finalizadas">Finalizadas</option>
                    </select>
                </div>
            </div>
        </form>

        <ul class="list-group mt-4">
            <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
                <input @change="evento => tarefa.finalizada = evento.target.checked" :id="tarefa.titulo" :checked="tarefa.finalizada" type="checkbox">
                <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
                    {{ tarefa.titulo }}
                </label>
            </li>
        </ul>
    </div>
</template>

<style scoped>
.done{
    text-decoration: line-through;
}
</style>
