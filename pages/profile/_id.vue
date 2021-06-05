<template>
<div class="container">
  <div class="row">
    <div class="col-lg-7">
      <div class="card shadow">
        <div class="card-body">
         <div class="row">
            <div class="col-lg-4">
            <!-- <img src="~/assets/images/avatar.svg" class=" avatar"> -->
          <img :src="profile.gender == 1 ? require('~/assets/images/avatar.svg') : require('~/assets/images/avatar_f.svg')" class=" avatar">

          </div>
            <div class="col-lg">
              <form method="post" @submit.prevent="updateUser">
                <div class="mb-3">
                  <label for="name" class="form-label">Name</label>
                  <input type="text" class="form-control" id="name" v-model="profile.name">
                </div>

                <div class="mb-3">
                  <label for="email" class="form-label">Email</label>
                  <input type="email" class="form-control" id="email" v-model="profile.email">
                </div>

                <div class="mb-3">
                  <label for="gender" class="form-label">Gender</label>
                  <select id="gender" class="form-control" v-model="profile.gender">
                    <option value="1">Laki-Laki</option>
                    <option value="0">Perempuan</option>
                  </select>
                </div>

                <button type="submit" class="btn btn-primary btn-sm">Update</button>
                <nuxt-link to="/profile" class="btn btn-warning btn-sm text-white">Cancel</nuxt-link>
              </form>
            </div>
         </div>
        </div>
      </div>
    </div>

    <div class="col-lg-5">
      <div class="card shadow">

        <div class="card-body">
          <h5 class="card-title font-monospace">Your Activity</h5>
        </div>
      </div>
    </div>
  </div>
</div>
</template>
<script>
import moment from 'moment'

export default {
  middleware: 'auth',
  data() {
    return {
      profile: [],
      errors: null
    }
  },

  mounted() {
    this.getUser()
  },

  methods: {
    updateUser() {
      this.$axios.put(`update-profile/${this.$route.params.id}`, {
        name: this.profile.name,
        email: this.profile.email,
        gender: this.profile.gender
      })
      .then(() => {
        this.$router.push('/profile')
      })
    },
    getUser(){
      this.$axios.get('me')
      .then(response => {
        this.profile = response.data.user
      })
    }
  },

  filters: {
    moment: function (date) {
      return moment(date).format('D MMMM YYYY');
    }
  }
}
</script>
<style>
.container {
  min-height: 100vh;
}

.avatar {
  height: 200px;
  width: 200px;
  /* display: block;
  margin-left: auto;
  margin-right: auto; */
}
</style>