<template>
  <div>
    <Header id="header"/>
    <api-form id="api"/>
  </div>
</template>

<script>
import Header from './components/header.vue'
import ApiForm from './components/api-form.vue'

export default {
  name: 'app',
  data() {
    return {
      history: []
    }
  },
  components: {
    Header,
    ApiForm
  }
}
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

#header {
  margin-bottom: 1em;
}
</style>
