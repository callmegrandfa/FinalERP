<template>
  <div id="app">
    <app-header></app-header>
    <div class="wrapper">
      <app-sidebar></app-sidebar>
      <app-content></app-content>
    </div>
    
  </div>
</template>

<script>
import header from './components/header'
import sidebar from './components/sidebar'
import content from './components/content'

export default {
  name: 'app',
  components: {
    'app-header':header,
    'app-sidebar':sidebar,
    'app-content':content,
  }
}
</script>
<style scoped>
.wrapper{
  background-color: rgba(38, 52, 75, 1);
  padding-top: 73px;
  position: relative;
  overflow: hidden;
}
</style>

