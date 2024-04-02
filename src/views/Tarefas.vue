<template>
  <Formulario @aoSalvarTarefa="salvarTarefa" />

  <div class="lista">
    <Tarefa v-for="(tarefa, index) in tarefas" :tarefa="tarefa" :key="index" />

    <Box v-if="listaEstaVazia">
      Você não está muito produtivo hoje :(
    </Box>

    <div class="modal is-active">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Modal title</p>
          <button class="delete" aria-label="close"></button>
        </header>
        <section class="modal-card-body">
          <!-- Content ... -->
        </section>
        <footer class="modal-card-foot">
          <div class="buttons">
            <button class="button is-success">Save changes</button>
            <button class="button">Cancel</button>
          </div>
        </footer>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue';
import Formulario from '../components/Formulario.vue';
import Tarefa from '../components/Tarefa.vue';
import Box from '../components/Box.vue';
import { useStore } from '@/store';
import { OBTER_TAREFAS, CADASTRAR_TAREFA, OBTER_PROJETOS } from '@/store/tipo-acoes';
import ITarefa from "@/interfaces/ITarefa";

export default defineComponent({
  name: 'App',
  components: { Formulario, Tarefa, Box },
  computed: {
    listaEstaVazia(): boolean {
      return this.tarefas.length === 0
    }
  },
  methods: {
    salvarTarefa(tarefa: ITarefa) {
      this.store.dispatch(CADASTRAR_TAREFA, tarefa)
    },
  },
  setup() {
    const store = useStore();
    store.dispatch(OBTER_TAREFAS)
    store.dispatch(OBTER_PROJETOS)

    return {
      tarefas: computed(() => store.state.tarefas),
      store
    }
  }
});
</script>