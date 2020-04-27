<template>
  <div class="container">
    <h3>Se senaste snippets</h3>
    <button @click="getLatestSnippets">Hämta</button>
    <p class="class-wait" v-if="waiting">Väntar på svar...</p>
    <br />
    <div class="class-message">
      <div class="class-clickable">
        <p @click="voteSuccess = false" v-show="voteSuccess">Du har röstat! &#128079;</p>
        <p @click="deleteSuccess = false" v-show="deleteSuccess">Du har tagit bort en snippet!</p>
        <p
          @click="unreportSuccess = false"
          v-show="unreportSuccess"
        >Du har nu ångrat din rapportering&#128524;</p>
      </div>
      <p v-show="reportSuccess && regret">Du har rapporterat en snippet!</p>
      <p
        class="class-regret"
        v-show="reportSuccess && regret"
      >Vill du kunna ångra din rapportering? &#128561;</p>
      <button v-show="reportSuccess" @click="showRegretButton = true">Ja</button>
    </div>
    <div class="class-div" v-for="snipp in latestSnippets" :key="snipp.id">
      <h3>{{snipp.title}}</h3>
      <code class="code">{{snipp.content}}</code>
      <br />
      <p>Antal röster: {{snipp.score}}</p>
      <p>Id: {{snipp.id}}</p>
      <button @click="voteFunction(snipp.id)">Rösta</button>
      <button @click="deleteFunction(snipp.id)">Ta bort</button>
      <button @click="reportFunction(snipp.id)">Rapportera</button>
      <button
        class="class-message-button"
        v-show="showRegretButton"
        @click="unreportFunction(snipp.id)"
      >Ångra</button>
    </div>
  </div>
</template>
<script>
const axios = require("axios");
export default {
  data() {
    return {
      latestSnippets: {},
      voteSuccess: false,
      deleteSuccess: false,
      reportSuccess: false,
      unreportSuccess: false,
      waiting: false,
      regret: false,
      showRegretButton: false
    };
  },
  methods: {
    getLatestSnippets() {
      this.waiting = true;
      axios
        .get(
          "https://www.forverkliga.se/JavaScript/api/api-snippets.php?latest"
        )
        .then(response => {
          console.log(response.data);
          this.latestSnippets = response.data;
          this.waiting = false;
        })
        .catch(error => {
          console.log("Something went wrong", error);
        });
    },
    voteFunction(snipp_id) {
      this.waiting = true;
      axios
        .post("https://www.forverkliga.se/JavaScript/api/api-snippets.php", {
          upvote: "",
          id: snipp_id
        })
        .then(response => {
          console.log(response.data);
          if (response) {
            this.waiting = false;
            this.voteSuccess = true;
          }
        })
        .catch(error => {
          console.log("Something went wrong...", error);
        });
    },
    deleteFunction(snipp_id) {
      this.waiting = true;
      axios
        .post("https://www.forverkliga.se/JavaScript/api/api-snippets.php", {
          delete: "",
          id: snipp_id
        })
        .then(response => {
          console.log(response.data);

          if (response) {
            this.deleteSuccess = true;
            this.waiting = false;
          }
        })
        .catch(error => {
          console.log("Something went wrong...", error);
        });
    },
    reportFunction(snipp_id) {
      this.voteSuccess = false;
      this.waiting = true;
      axios
        .post("https://www.forverkliga.se/JavaScript/api/api-snippets.php", {
          report: "",
          id: snipp_id
        })
        .then(response => {
          console.log(response.data);
          this.reportSuccess = true;
          this.waiting = false;
          this.regret = true;
        })
        .catch(error => {
          console.log("Något gick fel...", error);
        });
    },
    unreportFunction(snipp_id) {
      this.regret = false;
      this.waiting = true;
      axios
        .post("https://www.forverkliga.se/JavaScript/api/api-snippets.php", {
          unreport: "",
          id: snipp_id
        })
        .then(response => {
          console.log(response.data);
          this.unreportSuccess = true;
          this.waiting = false;
          this.reportSuccess = false;
          this.showRegretButton = false;
        })
        .catch(error => {
          console.log("Något gick fel...", error);
        });
    }
  }
};
</script>
<style scoped>
button :hover {
  opacity: 0.7;
}
.container {
  background-color: #aacfcf;
  margin-top: 10px;
  padding: 10px;
  color: #2c3e50;
}
.class-message-button {
  background-color: rgb(241, 241, 83);
}
.class-message {
  font-size: 1em;
}
.class-wait {
  cursor: wait;
}
.class-clickable :hover {
  opacity: 0.7;
}
.class-div {
  background-color: #fde2e2;
  border-radius: 10px;
  width: 30%;
  height: 50%;
  font-size: 1.2em;
  padding: 5px;
  margin: 10px auto;
}
.class-regret {
  font-size: 0.7em;
}
.code {
  font-family: "Courier New", Courier, monospace;
  font-style: italic;
  font-weight: bold;
  width: 100%;
}
</style>
