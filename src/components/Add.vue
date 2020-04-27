<template>
  <div class="container">
    <h3>Skapa en code snippet</h3>
    <form class="form-class">
      <input type="text" placeholder="Titel" v-model="snippet.title" id="title" name="title" />
      <br />
      <br />
      <textarea
        v-model="snippet.content"
        placeholder="Skriv din kod här"
        name="code"
        id="code"
        cols="20"
        rows="5"
        class="input-field"
      ></textarea>
    </form>
    <button @click="addSnippet">Spara</button>
    <p class="class-wait" v-show="waiting">Väntar på svar...</p>
    <p v-show="succeeded">Du lyckades spara! &#128079;</p>
    <p v-show="error">Något gick fel. Prova igen!</p>
  </div>
</template>
<script>
const axios = require("axios");
export default {
  data() {
    return {
      snippet: {
        title: "",
        content: ""
      },
      waiting: false,
      succeeded: false,
      error: false
    };
  },
  methods: {
    addSnippet() {
      this.waiting = true;
      axios
        .post("https://www.forverkliga.se/JavaScript/api/api-snippets.php", {
          add: "",
          title: this.snippet.title,
          content: this.snippet.content
        })
        .then(response => {
          console.log(response.data.message);
          this.snippet = {
            title: response.data.title,
            content: response.data.content
          };
          if (response) {
            this.succeeded = true;
            this.waiting = false;
          } else {
            this.error = true;
          }
        })
        .catch(error => {
          console.log("Something went wrong", error);
        });
    }
  }
};
</script>
<style scoped>
.input-field {
  border-radius: 3px;
  background-color: rgb(255, 255, 255);
}
.container {
  background-color: #fde2e2;
  margin-top: 20px;
  margin-bottom: 20px;
  padding: 20px;
  height: 250px;
  color: #2c3e50;
}
.class-wait {
  cursor: wait;
}
</style>
