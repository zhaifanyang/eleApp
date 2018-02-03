<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab">
      <div class="tab-item"></div>
      <div class="tab-item"></div>
      <div class="tab-item"></div>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  display: flex;
}
</style>
