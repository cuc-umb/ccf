<template>
  <div id="app">
    <h1>Форма создания клиента</h1>
    <FormComponent />
  </div>
</template>

<script>
import FormComponent from './components/formComponent.vue'

export default {
  name: 'App',
  components: {
    FormComponent
  }
}
</script>

<style lang="sass">
#app 
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px

</style>
