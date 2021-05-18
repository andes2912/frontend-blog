<template>
  <div class="container container-fluid">
    <div class="card-body shadow">
      <h5 class="font-monospace">{{errors == 404 ? 'Penulis Tidak Ditemukan' : 'Posted By ' + user.name}}</h5>
    </div>
    <div class="row">
      <div class="col-md-8 mt-2">
        <div v-if="errors == 404">
            <img src="~/assets/images/not_found.svg" class="images" />
        </div>
        <div v-else>
          <div class="row">
            <div class="col-md-6 mb-2" v-for="(berita, index) in news" :key="index">
              <nuxt-link :to="'/' + berita.slug" style="text-decoration:none">
                <div class="card shadow">
                  <div class="card-body">
                    <blockquote class="blockquote mb-0">
                    <p class="font-monospace fs-6">{{berita.title}}</p>
                      <footer class="blockquote-footer"> | {{berita.created_at |  moment}} </footer>
                    </blockquote>
                  </div>
                </div>
              </nuxt-link>
            </div>
          </div>
        </div>
      </div>

      <div class="col-md-4 mt-2">
        <div class="card shadow mt-2">
          <div class="card-body">
            <h4 class="font-monospace">Kategori</h4>
              <div class="d-flex flex-wrap bd-highlight mb-3">
                <div class="p-2 bd-highlight" v-for="(categori, index) in category" :key="index">
                  <nuxt-link :to="'/kategori/' + categori.slug" style="text-decoration:none; color:#577778">{{categori.name}}</nuxt-link>
                </div>
              </div>
          </div>
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
      news: [],
      user: [],
      category: [],
      errors: []
    }
  },

  filters: {
    moment: function (date) {
      return moment(date).format('D MMMM YYYY');
    }
  },

  mounted() {
    this.getNews(),
    this.getCategory()
  },

  methods: {
    getNews() {
      this.$axios.get(`news-by-name/${this.$route.params.name}`)
      .then(response => {
        this.news = response.data.data.news
        this.user = response.data.data
      })
      .catch(e => {
        this.errors = e.response.data.errors
      })
    },

    getCategory() {
      this.$axios.get('category')
      .then(response => {
        this.category = response.data.data
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
  margin-top: 2%;
}
</style>