<template>
  <div>
    <MyPreloader />
  </div>

</template>

<script>
import MyPreloader from '@/components/MyPreloader'

export default {
  name: 'App',
  components: {
    MyPreloader
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
