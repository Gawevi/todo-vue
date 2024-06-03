<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar',
        finalizada: false
      },
      {
        titulo: 'Escrever',
        finalizada: false
      },
      {
        titulo: 'Arrumar a casa',
        finalizada: true
      }
    ]
  })

  const getTarefasPendentes = () => {
     return estado.tarefas.filter(tarefa => tarefa.finalizada === false)
  }

  const getTarefasFinalizadas = () => {
     return estado.tarefas.filter(tarefa => tarefa.finalizada === true)
  }

  const getTarefasFiltradas = () => {
    const filtro = estado.filtro;

    switch(filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }

  const cadastrarTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastrarTarefa" />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>
