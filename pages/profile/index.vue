<template>
 <div class="container">
  <div class="row">
    <div class="col-lg-7">
      <div class="card shadow">
        <div class="card-body">
          <img :src="user.gender == 1 ? require('~/assets/images/avatar.svg') : require('~/assets/images/avatar_f.svg')" class=" avatar">
          <p class="font-monospace text-center mt-1">
            {{user.name}} </br>
            {{user.email}} <br>
            {{user.created_at | moment}} <span class="form-text">Join Date</span> <br>
            <nuxt-link :to="'profile/' + user.id" class="btn btn-primary btn-sm mt-1">Edit Profile</nuxt-link>
          </p>
        </div>
      </div>
    </div>

    <div class="col-lg-5">
      <div class="card shadow">

        <div class="card-body">
          <h5 class="card-title font-monospace">Your Activity</h5>
          <ul v-for="(logs, indes) in log" :key="index">
            <li> {{logs.method}} - {{logs.Note}}  | {{logs.created_at | moments}} </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import moment from 'moment'
import { mapGetters } from 'vuex'

export default {
  middleware: 'auth',

  data() {
    return {
      user: [],
      log: []
      }
  },

  mounted() {
    this.getUser(),
    this.getActivity()
  },

  methods: {
    getUser(){
      this.$axios.get('me')
      .then(response => {
        this.user = response.data.user
      })
    },

    getActivity() {
      this.$axios.get('log-activity')
      .then(response => {
        this.log = response.data.log
      })
    }
  },

  filters: {
    moment: function (date) {
      return moment(date).format('D MMMM YYYY');
    },

    moments: function (date) {
      return moment(date).format('D MMMM YYYY H:m:s')
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
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.biodata {

}
</style>