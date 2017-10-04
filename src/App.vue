<template>
  <div id="app" class="container">
    <notifications position="top center" type="warning" group="foo" class="errors"/>
    <Gmapp></Gmapp>
  </div>
</template>

<script>

if (localStorage.getItem("vuedata") === null) {
  localStorage['vuedata'] = "[]";
}

import Gmapp from './components/Gmapp';


export default {
  name: 'app',
  components: {
    Gmapp
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

.errors{
  margin: 10px;
}
</style>
