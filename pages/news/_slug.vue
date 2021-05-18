<template>
  <div class="container container-fluid">
    <div class="card shadow mb-2">
      <div class="card-body">
        <h5 class="font-monospace">Form Edit News</h5>
      </div>
    </div>
    <div class="card shadow">
      <div class="card-body">
        <form method="post" @submit="updateNews">
          <div class="row">
            <div class="alert alert-danger" v-if="error == 403">
              Kamu Tidak Bisa Mengedit Post Ini !
            </div>
            <div class="alert alert-danger" v-for="(error, index) in errors" :key="index">
              {{ error[0] }}
            </div>
            <div class="col-8">
              <div class="mb-3">
                <label for="title" class="form-label">Title</label>
                <input type="text" class="form-control" v-model="detail.title" id="title" placeholder="Enter Ttile">
              </div>
              <div class="mb-3">
                <label for="exampleFormControlTextarea1" class="form-label">Content</label>
                <no-ssr>
                  <quill-editor v-model='detail.content' ref='textEditor'></quill-editor>
                </no-ssr>
              </div>
            </div>
            <div class="col-3">
              <div class="mb-3">
                <label for="category" class="form-label">Category</label>
                <select class="form-select" v-model="category_id" required>
                  <option v-for="(categori, index) in category" :value="categori.id">{{categori.name}}</option>
                </select>
              </div>
            </div>
          </div>
          <button type="submit" class="btn btn-primary">Simpan</button>
          <nuxt-link class="btn btn-warning text-white" to="/news">Cancel</nuxt-link>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      detail: [],
      category: [],
      category_id: '',
      errors: null,
      error: null

    }
  },

  mounted() {
    this.getNewsDetail(),
    this.getCategory()
  },

  methods: {
    getNewsDetail() {
      this.$axios.get(`news/${this.$route.params.slug}`)
      .then(response => {
        this.detail = response.data.data
      })
      .catch(e => {
        console.log(e);
      })
    },
    updateNews(e) {
      e.preventDefault()

      this.$axios.put(`news/${this.$route.params.slug}`, {
        title:        this.detail.title,
        content:      this.detail.content,
        category_id:  this.category_id,
      })
      .then(() => {
        this.$router.push('/news')
      })
      .catch(e => {
        this.errors = e.response.data.errors
        this.error = e.response.data.error
        if ( this.error) {
          setTimeout( () => this.$router.push('/news'), 3000);
        }
      })
    },
    getCategory() {
      this.$axios.get('category')
      .then(response => {
        this.category = response.data.data
      })
      .catch(error => {
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
</style>