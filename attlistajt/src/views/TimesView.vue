<script>
import TimesApi from "@/api/times.js";
const timesApi = new TimesApi();
export default {
  data() {
    return {
      time: {},
      times: [],
    };
  },
  async created() {
    this.times = await timesApi.buscarTodosOsTimes();
  },
  methods: {
    async salvar() {
      if (this.time.id) {
        await timesApi.atualizarTime(this.time);
      } else {
        await timesApi.adicionarTime(this.time);
      }
      this.times = await timesApi.buscarTodosOsTimes();
      this.time = {};
    },
    async excluir(time) {
      await timesApi.excluirTime(time.id);
      this.times = await timesApi.buscarTodosOsTimes();
    },
    editar(time) {
      Object.assign(this.time, time);
    },
  },
};
</script>

<template>
  <main>
    <div class="container">
      <div class="title">
        <h2>Gerenciamento de Times</h2>
      </div>
      <div class="form-input">
        <input type="text" v-model="time.nome" placeholder="Time" />
        <input type="text" v-model="time.cidade" placeholder="Cidade" />
        <button @click="salvar">Adicionar</button>
      </div>
      <div class="list-times">
        <table>
          <thead>
            <tr>
              <th>ID</th>
              <th>Nome</th>
              <th>cidade</th>
              <th>Ações</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="time in times" :key="time.id">
              <td>{{ time.id }}</td>
              <td>{{ time.nome }}</td>
              <td>{{ time.cidade }}</td>
              <td>
                <button @click="editar(time)">Editar</button>
                <button @click="excluir(time)">Excluir</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </main>
</template>

<style>
.title {
  display: flex;
  justify-content: center;
  margin-top: 30px;
}

.form-input {
  margin-top: 10px;
  display: flex;
  justify-content: center;
}

.form-input input {
  width: 50px;
  height: 40px;
  border: 1px solid rgb(147, 147, 147);
  border-radius: 10px;
  padding: 0 10px;
}

.form-input button {
  margin-left: 1px;
  width: 20%;
  height: 40px;
  border: 1px solid rgb(211, 211, 211);
  border-radius: 10px;
  background-color: rgb(227, 170, 0);
  font-weight: bold;
  color: black;
  cursor: pointer;
}

.list-times {
  display: flex;
  justify-content: center;
}

.list-times table {
  width: 80%;
  margin: 0 auto;
  border-collapse: collapse;
}

table,
tr,
th,
td {
  border: 2px solid black;
  padding: 10px;
}
</style>
