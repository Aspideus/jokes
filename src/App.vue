<template>
  <JokeLoad msg="Jokes for folks"/>
</template>

<script>
import JokeLoad from './components/JokeLoad.vue'

export default {
  name: 'App',
  components: {
    JokeLoad
  },
  created: function () {
  }
}
</script>

<style>
:root {
    --color-primary: #6B7E89;
    --color-bg: #f0f0f0;
    --color-for-cards: #FAF7F3;
    --color-font: #000;
    --color-liked: #42b983;
}
body {
  background-color: var(--color-bg);
  color: var(--color-font);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
