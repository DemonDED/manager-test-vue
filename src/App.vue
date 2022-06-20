<template>
  <div id="app">
    <div class="head-manager">
      <h1>Приложение "Менеджер"</h1>
      <hr>
    </div>
      <div class="content-manager">
        <table-manager />
      </div>
  </div>
</template>

<script>
import TableManager from './components/TableManager.vue'

export default {
  data () {
    return {
    }
  },
  mounted () {
    console.log('Hello Vue')
  },
  components: {
    TableManager
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

</style>
