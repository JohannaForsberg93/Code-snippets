<template>
  <div class="container">
    
      <h3>Se senaste snippets</h3>
      <br>
      <button @click="getLatestSnippets">Hämta</button>
      <p v-if="waiting">Väntar på svar...</p>
      <br>
      <div class="class-message">
        <p v-if="voteSuccess">Du har röstat! &#128079; </p>
        <p v-if="deleteSuccess">Du har tagit bort en snippet!</p>
        <div>
          <p v-show="regret" v-if="reportSuccess">Du har rapporterat en snippet! Vill du ångra? &#128561;</p>
          <button v-if="reportSuccess" @click="unreportFunction(snipp_id)">Ångra</button>
          <p v-if="unreportSuccess">Du har nu ångrat din rapportering. &#128524;</p>
        </div>
      </div>

      <div class="class-div" v-for="snipp in latestSnippets" :key="snipp.id">
          <h3>{{snipp.title}}</h3>
          <code class="code">{{snipp.content}}</code><br>
          <p>Antal röster: {{snipp.score}}</p>
          <p>Id: {{snipp.id}}</p>
          <button @click="voteFunction(snipp.id)">Rösta</button>
          <button @click="deleteFunction(snipp.id)"> Ta bort</button>
          <button @click="reportFunction(snipp.id)">Rapportera</button>
              
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
    

    };
  },

  methods: {
    
    getLatestSnippets(){
        this.waiting = true;
        axios
        .get(
          "https://www.forverkliga.se/JavaScript/api/api-snippets.php?latest"
        )

        .then(response => {
          console.log(response.data);
          this.latestSnippets = response.data
          this.waiting = false;


        })

        .catch(error => {
          console.log("Something went wrong", error);
        });
  },

  voteFunction(snipp_id){
    this.waiting = true;
    axios.post(
          "https://www.forverkliga.se/JavaScript/api/api-snippets.php", {upvote:'', id: snipp_id}
        )

        .then(response => {
          console.log("Nu är jag i vote-funktionen.")
          console.log(response.data);

          if(response){
            this.waiting = false;
            this.voteSuccess = true;
           
        }

        })
        .catch(error => {
          console.log("Something went wrong", error);
        });
  },

  deleteFunction(snipp_id){
    this.waiting = true;
  
    axios.post(
          "https://www.forverkliga.se/JavaScript/api/api-snippets.php", {delete:'', id: snipp_id}
        )
        .then(response => {
          console.log("Nu är jag i delete-funktionen.")
          console.log(response.data);

          if(response){
            this.deleteSuccess = true;
            this.waiting = false;
          }

        })
        .catch(error => {
          console.log("Något gick fel...", error);
        });
      },
      reportFunction(snipp_id){
        this.waiting = true;
        axios.post(
          "https://www.forverkliga.se/JavaScript/api/api-snippets.php", {report:'', id: snipp_id}
        )
        .then(response => {
          console.log("Nu är jag i report-funktionen.")
          console.log(response.data);
          this.reportSuccess = true;
          this.waiting = false;
          this.regret = true;

        })
        .catch(error => {
          console.log("Något gick fel...", error);
        });

      },
      
      unreportFunction(snipp_id){
        this.regret = false;
        this.waiting = true;
        axios.post(
          "https://www.forverkliga.se/JavaScript/api/api-snippets.php", {unreport:'', id: snipp_id}
        )
        .then(response => {
          console.log("Nu är jag i unreport-funktionen.")
          console.log(response.data);
          this.unreportSuccess = true;
          this.waiting = false;

        })
        .catch(error => {
          console.log("Något gick fel...", error);
        });

      }

    }

}

</script>


<style scoped>

.container{
  background-color: #aacfcf;
  margin-top: 10px;
  padding: 10px; 
  color: #2c3e50;
}
.class-message button{
  background-color: rgb(241, 241, 83);
}
.class-message{
  font-size: 1em;
  text-decoration: underline;
}
.class-div{
  background-color: rgb(230, 210, 210);
  border-radius: 10px;
  width: 30%;
  height: 50%;
  font-size: 1.2em;
  padding: 5px;
  margin: 10px auto;
   
}
.code{
  font-family:'Courier New', Courier, monospace;
  font-style: italic;
  font-weight: bold;
  width: 100%;


}

</style>
