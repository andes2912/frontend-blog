//default.vue
<template>
  <div>
    <Navbar/>
    <br>
    <div class="text-center" v-if="$nuxt.isOffline">Anda sedang offline</div>
    <nuxt/>
    <Footer />
  </div>
</template>

<script>
import Navbar from '~/components/Navbar'
import Footer from '~/components/Footer'

export default {
  components: {
    Navbar, Footer
  }
}
</script>

<style>
/* .navbar-nav .nav-item .nuxt-link-active{
  color:black;
} */
</style>
