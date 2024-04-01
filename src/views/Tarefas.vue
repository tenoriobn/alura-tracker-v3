<template>
  <Formulario @aoSalvarTarefa="salvarTarefa" />

  <div class="lista">
    <Tarefa v-for="(tarefa, index) in tarefas" :tarefa="tarefa" :key="index" />

    <Box v-if="listaEstaVazia">
      Você não está muito produtivo hoje :(
    </Box>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  import Formulario from '../components/Formulario.vue';
  import Tarefa from '../components/Tarefa.vue';
  import ITarefa from '../interfaces/ITarefa'
  import Box from '../components/Box.vue';

  export default defineComponent({
      name: 'App',
      components: { Formulario, Tarefa, Box },
      data() {
        return {
          tarefas: [] as ITarefa[],
        }
      },
      computed: {
        listaEstaVazia() : boolean {
          return this.tarefas.length === 0
        }
      },
      methods: {
        salvarTarefa(tarefa: ITarefa) {
          this.tarefas.push(tarefa)
        },
      }
  });
</script>