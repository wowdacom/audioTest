<template>
  <div id="app">
    <helloVoice></helloVoice>
    <helloVoice></helloVoice>
    <helloVoice></helloVoice>
  </div>
</template>

<script>
import helloVoice from './components/HelloWorld.vue';

export default {
  name: 'app',
  components: {
    helloVoice,
  },
};
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
