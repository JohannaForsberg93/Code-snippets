<template>
  <div class="container">
    <h3>Här visas de bästa snippets</h3>
    <button @click="bestSnippetsFunction">Hämta</button>
    <p class="class-wait" v-if="waiting">Väntar på svar...</p>
    <br />
    <div class="class-div" v-for="snipp in bestSnippets" :key="snipp.id">
      <h3>{{snipp.title}}</h3>
      <code class="code">{{snipp.content}}</code>
      <br />
      <p>Antal röster: {{snipp.score}}</p>
      <p>Id: {{snipp.id}}</p>
      <p>&#11088; &#11088; &#11088; </p>
    </div>
  </div>
</template>

<script>
const axios = require("axios");
export default {
  data() {
    return {
      waiting: false,
      bestSnippets: {}
    };
  },

  methods: {
    bestSnippetsFunction() {
      this.waiting = true;
      axios
        .get("https://www.forverkliga.se/JavaScript/api/api-snippets.php?best")
        .then(response => {
          console.log("Inuti bestSnippet-funktionen", response.data);
          this.bestSnippets = response.data;
          this.waiting = false;
        })
        .catch(error => {
          console.log("Something went wrong", error);
        });
    }
  }
};
</script>

<style scoped>
.container {
  background-color: rgb(166, 150, 187);
  margin-top: 10px;
  padding: 10px;
  color: #2c3e50;
}
.class-div {
  background-color:  #fde2e2;
  border-radius: 10px;
  width: 30%;
  height: 50%;
  font-size: 1.2em;
  padding: 5px;
  margin: 10px auto;
}
.class-code {
  font-family: "Courier New", Courier, monospace;
  font-style: italic;
  font-weight: bold;
  width: 100%;
}
.class-wait{
    cursor: wait;
}
</style>