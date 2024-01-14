<script setup>
import { reactive } from "vue";

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizado: false,
    },
    {
      titulo: 'Fazer o projeto do curso',
      finalizado: false,
    },
    {
      titulo: 'Ir pra academia',
      finalizado: true,
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizado)
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizado)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes()
    case 'finalizadas':
      return getTarefasFinalizadas()
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizado: false,
  }
  estado.tarefas.push(tarefaNova)
  estado.tarefaTemp = ''
}

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <div class="container-fluid py-5 text-center">
        <h1 class="display-5 fw-bold">Minhas tarefas</h1>
        <p class="col-md-8 mx-auto">Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
      </div>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row g-3 align-items-center">
        <div class="col">
          <input @input="evento => estado.tarefaTemp = evento.target.value" required type="text" id="taskDescription"
            class="form-control" placeholder="Digite a descrição da tarefa">
        </div>
        <div class="col-md-auto">
          <button type="submit" class="btn btn-primary mb-3 ms-3">Cadastrar</button>
        </div>
        <div class="col-md-auto">
          <select class="form-select ms-3" aria-label="Default select example"
            @change="evento => estado.filtro = evento.target.value">
            <option selected>Escolha...</option>
            <option value="todas">Todas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    {{ estado.filtro }}
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="(tarefa, index) in getTarefasFiltradas()" :key="index">
        <input @change="evento => tarefa.finalizado = evento.target.checked" type="checkbox" v-model="tarefa.finalizado"
          :id="tarefa.titulo">
        <label :class="{ done: tarefa.finalizado === true }" class="ms-3" :for="tarefa.titulo">{{ tarefa.titulo
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
