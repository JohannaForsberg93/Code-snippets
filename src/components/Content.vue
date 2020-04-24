<template>
  <div class="container-class">
    <div class="content-class">
      <h2>Lägg till eller visa snippets</h2>
      <button @click="showAddComponent">Lägg till Snippet</button>
      <button @click="showLatestComponent">Senaste Snippet</button>
      <Add v-show="selected == 'add' " />
      <Latest v-show="selected == 'latest' " />
    </div>
  </div>
</template>
<script>
import Add from "./Add";
import Latest from "./Latest";

export default {
  components: {
    Add,
    Latest
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
    }
  }
};
</script>
<style scoped>
.container-class {
  text-align: center;
  margin-top: 20px;
  margin: 10px;
  color: white;
}
.content-class {
  height: 100%;
  background-color: #679b9b;
  margin: 20px;
  padding: 20px;
  color: white;
}
</style>