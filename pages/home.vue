<template>
  <div class="jumbotron container">
    <h1 class="display-4">Dashboard</h1>
  </div>
</template>
<script>
export default {
  middleware: 'auth'
}
</script>
<style>
.container {
  min-height: 100vh;
}
</style>