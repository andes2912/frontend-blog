<template>
  <div class="container container-fluid">
    <div class="card-body shadow">
      <h5 class="font-monospace">Kategori {{errors == 404 ? 'Tidak Ditemukan' : name_category.name}}</h5>
    </div>
    <div class="row">

      <div class="col-lg-8">
        <div v-if="errors == 404">
          <img src="~/assets/images/not_found.svg" class="images" />
        </div>
        <div v-else-if="nulls <= 0">
          <img src="~/assets/images/post.svg" class="images-post" />
        </div>
        <div class="row" v-else>
            <div class="col-md-6 col-sm-6 mt-2" v-for="(kategori, index) in category" :key="index">
              <nuxt-link :to="'/' + kategori.slug" style="text-decoration:none">
                <div class="card shadow">
                  <div class="card-body">
                    <blockquote class="blockquote mb-0">
                      <p class="font-monospace fs-6">{{kategori.title}}</p>
                      <footer class="blockquote-footer"> {{name_category.name}} | {{kategori.created_at |  moment}} </footer>
                    </blockquote>
                  </div>
                </div>
              </nuxt-link>
            </div>
        </div>
      </div>
      <div class="col-lg-4 mt-2">
       <div class="card shadow mt-2">
          <div class="card-body">
            <h4 class="font-monospace">Kategori</h4>
             <div class="d-flex flex-wrap bd-highlight mb-3">
                <div class="p-2 bd-highlight" v-for="(categori, index) in categori_all" :key="index">
                  <nuxt-link :to="'' + categori.slug" style="text-decoration:none; color:#577778">{{categori.name}}</nuxt-link>
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
      category: [],
      name_category: [],
      categori_all: [],
      nulls: [],
      errors: []
    }
  },

  filters: {
    moment: function (date) {
      return moment(date).format('D MMMM YYYY');
    }
  },

  mounted() {
    this.getCategory(),
    this.getCategoryAll()
  },

  methods: {
    getCategory() {
      this.$axios.get(`category/${this.$route.params.slug}`)
      .then(response => {
        this.category = response.data.data.news
        this.name_category = response.data.data
        this.nulls = response.data.data.news.length

      })
      .catch(e => {
        this.errors = e.response.data.errors
      })
    },

    getCategoryAll() {
      this.$axios.get('category')
      .then(response => {
        this.categori_all = response.data.data
      })
    }
  }
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
.images-post {
  max-width: 90%;
  max-height: 80%;
  position: relative;
  margin-top: 2%;
}
</style>