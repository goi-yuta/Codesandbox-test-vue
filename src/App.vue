<template>
  <div id="app">
    <HelloWorld v-bind:play="play" v-on:reset="playToggle()" />
    <button v-on:click="playToggle()">START</button>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  data() {
    return {
      play: false,
    };
  },
  components: {
    HelloWorld,
  },
  methods: {
    playToggle() {
      this.play = !this.play;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
