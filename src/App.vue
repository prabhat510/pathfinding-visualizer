<template>
  <div id="app">
    <PathfindingVisualizer :key="value" @clear="clear" />
  </div>
</template>

<script>
import PathfindingVisualizer from "./components/PathfindingVisualizer.vue";

export default {
  name: "App",
  data() {
    return { value: 0 };
  },
  components: {
    PathfindingVisualizer,
  },
  methods: {
    clear() {
      this.value += 1;
    },
  },
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
