<template>
  <div>
    <LoaderCircle />
    <dual-list/>
    <loader-line />
  </div>

</template>

<script>
import LoaderCircle from '@/components/LoaderCircle'
import LoaderLine from '@/components/LoaderLine'
import DualList from './components/DualList.vue'

export default {
  name: 'App',
  components: {
    LoaderCircle,
    DualList,
    LoaderLine
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
