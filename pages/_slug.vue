<template>
  <div class="container">
    <div class="row">
      <div class="col-md-8 mb-4">
        <div v-if="errors == 404">
          <img src="~/assets/images/not_found.svg" class="images" />
        </div>
        <div v-else>
          <div class="card shadow">
            <div class="card-body">
              <p class="font-monospace">{{detail.title}}</p>
              <footer class="blockquote-footer fst-italic"> <nuxt-link :to="'kategori/' + category.slug" style="text-decoration:none">{{category.name}}</nuxt-link> | {{detail.created_at |  moment}} </footer>
              <p class="text-muted" v-html="detail.content"></p>
            </div>
          </div>
          <div class="col mt-1">
            <div class="card shadow">
              <div class="card-body">
                <span class="fw-lighter fst-italic fs-5"><nuxt-link :to="'news/user/' + user.name" style="text-decoration:none">{{user.name}}</nuxt-link></span> <br>
                <span class="fw-light fs-6">Penulis</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-4 mb-2">
        <div class="col mb-2" v-for="(berita, index) in news" :key="index">
          <nuxt-link :to="berita.slug" style="text-decoration:none">
            <div class="card shadow">
              <div class="card-body">
                <blockquote class="blockquote mb-0">
                  <p class="font-monospace fs-6">{{berita.title}}</p>
                  <footer class="blockquote-footer">{{berita.user.name}} | {{berita.created_at |  moment}}  </footer>
                </blockquote>
              </div>
            </div>
          </nuxt-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import moment from 'moment'

export default {
  data() {
    return {
      detail: [],
      user: [],
      category: [],
      news: [],
      errors: []
    }
  },

  filters: {
    moment: function (date) {
      return moment(date).format('D MMMM YYYY');
    }
  },

  mounted() {
    this.getNewsDetail(),
    this.getNews()
  },

  methods: {
    getNewsDetail() {
      this.$axios.get(`news/${this.$route.params.slug}`)
      .then(response => {
        this.detail = response.data.data
        this.user = response.data.data.user
        this.category = response.data.data.category
      })
      .catch(e => {
        this.errors = e.response.data.errors
        console.log(e.response.data.errors)
      })
    },

    getNews() {
      this.$axios.get('news')
      .then(response => {
        this.news = response.data.data
      })
      .catch(e => {
        this.errors = e.response.data.errors
      })
    }
  },
}
</script>
<style>
.container {
  min-height: 100vh;
}
.images {
  max-width: 100%;
  position: relative;
}
</style>