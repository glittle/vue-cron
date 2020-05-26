<template>
  <div id="app">
    <h2>Sample</h2>
    <vueCron :input="value" v-on:change="update"></vueCron>
    <p>
      Initial Input:
      <el-input v-model="value"></el-input>
    </p>
  </div>
</template>

<script>
import vueCron from "./vue-cron";

export default {
  name: "App",
  components: {
    vueCron
  },
  data() {
    return {
      value: "* * * * * * 2020-2022"
    };
  },
  methods: {
    update(v) {
      this.value = v;
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
    