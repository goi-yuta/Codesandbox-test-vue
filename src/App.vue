<template>
  <div id="app">
    <HelloWorld v-show="play" v-bind:play="play" v-on:reset="playToggle()" />
    <div class="overlay" v-show="!play">
      <button class="button" v-on:click="playToggle()">START</button>
    </div>
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
  position: relative;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 480px;
  height: 320px;
  text-align: center;
  color: #2c3e50;
}
.overlay {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
}
</style>
