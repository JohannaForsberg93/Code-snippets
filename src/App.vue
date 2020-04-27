<template>
  <div id="app">
    <Header />
    <Content />
  </div>
</template>

<script>
import Content from "./components/Content.vue";
import Header from "./components/Header.vue";
export default {
  name: "App",
  components: {
    Content,
    Header
  },
  data() {
    return {
      snippId: null
    };
  },
  methods: {
    test(id) {
      this.snippId = id;
    }
  }
};
</script>
<style>
h2,
h3 {
  color: #2c3e50;
}
button {
  display: inline-block;
  margin: 0;
  padding: 0.5em;
  border-radius: 8px;
  text-decoration: none;
  font-family: Arial;
  font-weight: 400;
  font-size: 15px;
  background-color: #ececec;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
