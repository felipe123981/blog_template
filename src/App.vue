<template>
  <div class="container">
    <h1>Digite aqui seu artigo:</h1>
    <input type="text" placeholder="Titulo" v-model="title" /><br />
    <textarea v-model="content" placeholder="Conteúdo do post..."></textarea
    ><br />
    <button @click="addTask">Postar</button>
  </div>
  <table>
    <thead>
      <h1>Posts recentes:</h1>
    </thead>
    <tbody>
      <tr v-for="artigo in artigos" v-bind:key="artigo.id">
        <td>
          <h3>{{ artigo.title }}</h3>
          <p>{{ artigo.content }}</p>
          <hr />
        </td>
        <td @click="removeTask(artigo.id)"><p class="close">X</p></td>
      </tr>
    </tbody>
  </table>
</template>
<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      artigos: [],
      title: "",
      content: "",
    };
  },
  methods: {
    async addTask() {
      
      this.artigos.push({
        id: this.artigos.id,
        title: this.title,
        content: this.content,
      });

      await axios
        .post("http://localhost:3333/posts", {
          "title": this.title,
          "content": this.content,
        })
        .then(function (response) {
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        });
      (this.title = ""), (this.content = "");
    },
    async removeTask(id) {
      for (var i = this.artigos.length; i--; ) {
        if (this.artigos[i].id === id) {
          this.artigos.splice(i, 1);
          await axios.delete("http://localhost:3333/posts/" + id);
        }
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3333/posts")
      .then((response) => (this.artigos = response.data));
  },
};
</script>
<style>
textarea {
  height: 70px;
  width: 370px;
}
table {
  border-spacing: 10px 10px;
}
tr {
  padding: 8px;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin: 20px;
}
.close {
  color: brown;
}
</style>
