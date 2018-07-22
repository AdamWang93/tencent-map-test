<template>
  <div id="app">
    <h2>Tencent Map Test</h2>
    <Map/>
  </div>
</template>

<script>
import Map from "./components/Map";

export default {
  name: "app",
  components: {
    Map
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100%;
}
</style>
