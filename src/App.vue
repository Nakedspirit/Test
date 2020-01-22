<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h2>Github Pull Request Monitor</h2>
    <prList />
  </div>
</template>
<script>
import prList from "./components/prList";
export default {
  name: "App",
  components: {
    prList
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
