<template>
  <div class="box formulario">
    <div class="columns">
      <div class="column is-5" role="form" aria-label="Formulário para criação de uma nova tarefa">
        <input 
          type="text" 
          class="input" 
          placeholder="Qual tarefa você deseja iniciar?"
          v-model="descricao"
        >
      </div>
      <div class="column is-3">
        <div class="select">
          <select v-model="idProjeto">
            <option value="">Selecione o projeto</option>
            <option
              v-for="projeto in projetos"
              :value="projeto.id"
              :key="projeto.id"
            >
              {{ projeto.nome }}
            </option>
          </select>
        </div>
      </div>

      <div class="column">
        <Temporizador @aoTemporizadorFinalizado="salvarTarefa" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent, computed, ref } from 'vue';
  import { useStore } from '@/store';
  import Temporizador from "./Temporizador.vue";
  import { NOTIFICAR } from '@/store/tipo-mutacoes';
  import { TipoNotificacao } from '@/interfaces/INotificacao';

  export default defineComponent({
    name: 'FormularioComponent',
    emits: ['aoSalvarTarefa'],
    components: {
      Temporizador
    },
    setup(props, { emit }) {
      const store = useStore();

      const descricao = ref("");
      const idProjeto = ref("");

      const projetos = computed(() => store.state.projeto.projetos);

      const salvarTarefa = (tempoEmSegundos: number) : void => {    
        const projeto = projetos.value.find((proj) => proj.id == idProjeto.value);

        if(!projeto) {
          store.commit(NOTIFICAR, {
            titulo: 'Ops!',
            texto: "Selecione um projeto antes de finalizar a tarefa!",
            tipo: TipoNotificacao.FALHA,
          });
          return; 
        }

        emit('aoSalvarTarefa', {
          duracaoEmSegundos: tempoEmSegundos,
          descricao: descricao.value,
          projeto: projeto
        })
        descricao.value = ''
      }

      return {
        descricao,
        idProjeto,
        projetos,
        salvarTarefa
      }
    }
  });
</script>

<style>
  .formulario {
    background-color: var(--bg-primario);
    color: var(--texto-primario);
  }
</style>
