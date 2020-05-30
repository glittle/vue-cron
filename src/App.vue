<template>
  <div id="app">
    <h2>Sample</h2>
    <vueCron v-model="value" :show-seconds="true" :extra="extra" :debug="debug"></vueCron>
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
      value: "0 0 0 1 1 * 2020-2022",
      extra: "test",
      debug: true
    };
  },
  methods: {
    // update(v) {
    //   this.value = v;
    // }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}
</style>
    