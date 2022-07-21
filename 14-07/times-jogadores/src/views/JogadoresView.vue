 <script>
import axios from 'axios';

export default {
  data() {
    return {
      novo_jogador: [],
      novo_time: [],
      jogadores: {}
    };
  },
  async created() {
    await this.buscarTodosOsJogadores();
    await this.buscarTodosOsTimes();
  },
  methods: {
    async buscarTodosOsTimes() {
      const times = await axios.get("http://localhost:4000/times");
      this.times = times.data;
    },
    async buscarTodosOsJogadores() {
      const jogadores = await axios.get(
        "http://localhost:4000/jogadores?expand=time"
      );
      this.jogadores = jogadores.data;
    },
    async salvar() {
      await axios.post("http://localhost:4000/jogadores", this.jogador);
      await this.buscarTodosOsJogadores();
    },
  },
};
</script>
<template>
 <div class="container">
  <div class="title">
    <h2>Gerenciamento de jogadores</h2>
  </div>
  <div class="form-input">
    <input type="text" v-model="jogador.nome" placeholder="Nome" />
    <input type="text" v-model="jogador.anoNascimento" placeholder="Ano Nascimento"/>
    <input type="text" v-model="jogador.posicaoJogo" placeholder="Posição de Jogo"/>
    <input type="text" v-model="jogador.altura" placeholder="Altura"/>
    <input type="text" v-model="jogador.peso" placeholder="Peso"/>
    <select v-model="jogador.timeId">
      <option v-for="time in times" :key="time.id" :value="time.id">
    {{ time.nome }}
      </option>
    </select>
    <button @click="salvar"> salvar</button>
  </div>
  
  <div class="list-itens">
    <table class="w-100">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Altura</th>
          <th>Peso</th>
          <th>Posição Jogo</th>
          <th>Idade</th>
          <th>Time</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="jogador in jogadores" :key="jogador.id">
          <td>{{jogador.id}}</td>
          <td>{{jogador.nome}}</td>
          <td>{{jogador.altura}}</td>
          <td>{{jogador.peso}}</td>
          <td>{{jogador.posicaoJogo}}</td>
          <td>{{jogador.anoNascimento}} anos</td>
          <td>{{jogador.time.nome}}</td>
          <td>???</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
</template>
<style scoped>
.w-100 {
  width: 100%;
}

</style> 