<template>
  <div id="app">
    <span id="title">Wynncraft Horse Breeding Simulator</span>
    <EmeraldsView></EmeraldsView>
    <HorseView></HorseView>
  </div>
</template>

<script>
import EmeraldsView from "./components/EmeraldsView.vue";
import HorseView from "./components/HorseView";

export default {
  name: "App",
  components: {
    EmeraldsView,
    HorseView
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
