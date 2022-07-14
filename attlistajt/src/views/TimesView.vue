<script>
import axios from "axios";
export default {
  data() {
    return {
      times: [],
      nova_cidade: [],
    };
  },
  async created(){
    const times = await axios.get("http://localhost:4000/times");
    this.times = times.data;
  },
  methods: {
    async salvar() {
      const time = {
        nome: this.novo_time,
        cidade: this.nova_cidade
      };
      const time_criado = await axios.post("http://localhost:4000/times", time);
      this.times.push(time_criado.data);
    },
    async excluir(time) {
      await axios.delete(`http://localhost:4000/times/${time.id}`);
      const indice = this.times.indexOF(time);
      this.times.splice(indice, 1);
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
        <input type="text" v-model="novo_time" />
        <input type="text" v-model="nova_cidade" />
        <button @click="salvar">Salvar</button>
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
                <button>Editar</button>
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
  width: 60%;
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
