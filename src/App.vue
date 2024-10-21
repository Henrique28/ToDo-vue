<script setup>

import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import Lista from './components/Lista.vue';

const estado = reactive({
    filtro: 'todas',
    tarefaTem: '',
    tarefas: []
});

const cadastraTarefa = () => {
    const tarefaNova = {
        titulo: estado.tarefaTem,
        finalizada: false,
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
    const { filtro } = estado;

    switch (filtro) {
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
        <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
        <Formulario :cadastro-tarefa="cadastraTarefa" :tarefa-tem="estado.tarefaTem" :edita-tarefa-temp="evento => estado.tarefaTem = evento.target.value" :trocar-filtro="evento => estado.filtro = evento.target.value"/>
        <Lista :tarefas="getTarefasFiltradas()" />
    </div>
</template>
