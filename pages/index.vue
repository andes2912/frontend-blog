<template>
  <div class="container">
    <div class="row">
      <div class="col-lg-8">
        <div class="row">
          <div v-if="cekNews == 0">
            <img src="~/assets/images/post.svg" class="images" />
          </div>
          <div class="col-lg-6 mb-2" v-for="(berita, index) in news" :key="index">
            <nuxt-link :to="berita.slug" style="text-decoration:none">
              <div class="card shadow">
                <div class="card-body">
                  <blockquote class="blockquote mb-0">
                  <p class="font-monospace fs-6">{{berita.title}}</p>
                    <footer class="blockquote-footer">{{berita.user.name}} | {{berita.created_at |  moment}} </footer>
                  </blockquote>
                </div>
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>

      <div class="col-lg-4">
        <div class="card shadow">
          <div class="card-body">
            <h4 class="font-monospace">Berita Populer</h4>
            <div class="col" v-for="(berita, index) in news" :key="index">
              <nuxt-link :to="berita.slug" style="text-decoration:none; color:#577778">{{berita.title}}</nuxt-link>
            </div>
          </div>
        </div>

        <div class="card shadow mt-2">
          <div class="card-body">
            <h4 class="font-monospace">Kategori</h4>
             <div class="d-flex flex-wrap bd-highlight mb-3">
                <div class="p-2 bd-highlight" v-for="(categori, index) in category" :key="index">
                  <nuxt-link :to="'kategori/' + categori.slug" style="text-decoration:none; color:#577778">{{categori.name}}</nuxt-link>
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
      category: [],
      cekNews:[],
      errors: null
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
      this.$axios.get('news')
      .then(response => {
        this.news = response.data.data
        this.cekNews = response.data.data.length
        console.log(response.data.data.length)
      })
      .catch(e => {
        this.errors = e.response.data
      })
    },
    getCategory() {
      this.$axios.get('category')
      .then(response => {
        this.category = response.data.data
      })
    }
  }
}
</script>

<style>
.container {
  /* margin: 0 auto; */
  min-height: 100vh;
}

.images {
  max-width: 100%;
  position: relative;
}
</style>
