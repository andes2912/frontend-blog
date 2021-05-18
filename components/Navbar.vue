//Navbar.vue
<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <nuxt-link class="navbar-brand" to="/">My Blog</nuxt-link>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <template v-if="isLoggedIn">
            <li class="nav-item">
              <nuxt-link class="nav-link" to="/home">Home</nuxt-link>
            </li>
            <li class="nav-item">
              <nuxt-link class="nav-link" to="/news">News</nuxt-link>
            </li>
            <li class="nav-item">
              <nuxt-link class="nav-link" to="/category">Category</nuxt-link>
            </li>
          </template>
        </ul>
        <ul class="navbar-nav">
          <template v-if="isLoggedIn">
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                {{user.name}}
              </a>
              <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdown">
                <li><nuxt-link class="dropdown-item" to="/profile">Profile</nuxt-link></li>
                <li><hr class="dropdown-divider"></li>
                <li><a class="dropdown-item" href="" @click="logout">Logout</a></li>
              </ul>
            </li>
          </template>
          <template v-else>
            <li class="nav-item">
              <nuxt-link class="nav-link" to="/login">Login</nuxt-link>
            </li>
            <li class="nav-item">
              <nuxt-link class="nav-link" to="/register">Register</nuxt-link>
            </li>
          </template>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  computed: {
    ...mapGetters([
        'user',
        'isLoggedIn'
    ])
  },
  methods: {
   async logout() {
    await this.$auth.logout();
   }
  }
}
</script>
