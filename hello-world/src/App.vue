<template>
  <div>
    <MyPreloader />
    <dual-list/>
  </div>

</template>

<script>
import MyPreloader from '@/components/MyPreloader'
import DualList from './components/DualList.vue'

export default {
  name: 'App',
  components: {
    MyPreloader,
    DualList
  }
}
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
