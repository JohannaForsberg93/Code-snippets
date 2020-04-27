<template>
  <div class="container-class">
    <div class="content-class">
      <h2>Lägg till eller visa snippets</h2>
      <button @click="showAddComponent">Lägg till Snippet</button>
      <button @click="showLatestComponent">Senaste Snippet</button>
      <button @click="showBestComponent">Bästa Snippet</button>
      <Add v-show="selected == 'add' " />
      <Latest v-show="selected == 'latest' " />
      <Best v-show="selected == 'best' " />
    </div>
  </div>
</template>
<script>
import Add from "./Add";
import Latest from "./Latest";
import Best from "./Best";

export default {
  components: {
    Add,
    Latest,
    Best
  },
  data: () => ({
    selected: ""
  }),
  methods: {
    showAddComponent() {
      this.selected = "add";
    },
    showLatestComponent() {
      this.selected = "latest";
    },
    showBestComponent() {
      this.selected = "best";
    }
  }
};
</script>
<style scoped>
.container-class {
  text-align: center;
  color: white;
}
.content-class {
  background-color: #679b9b;
  padding: 20px;
  color: white;
}
</style>