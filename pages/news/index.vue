<template>
  <div class="container-fluid container">
    <div class="card shadow mb-2">
      <div class="card-body">
        <div class="button col">
          <h5 class="font-monospace">News</h5>
          <nuxt-link to="/news/create" class="btn btn-primary font-monospace text-end">Add News</nuxt-link>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-12 col-sm-">
        <div class="card shadow">
          <div class="card-body">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Title</th>
                  <th scope="col">Category</th>
                  <th scope="col">Status</th>
                  <th scope="col">Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(berita, index) in news" :key="index">
                  <th scope="row">1</th>
                  <td>{{berita.title}}</td>
                  <td>{{berita.category.name}}</td>
                  <td>{{berita.deleted_at == NULL ? 'Publish' : 'UnPublish'}}</td>
                  <td>
                    <nuxt-link :to="'news/' + berita.slug" class="btn btn-info btn-sm text-white">Edit</nuxt-link>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

export default {
  middleware: 'auth',
  data() {
    return {
      news: []
    }
  },

  mounted() {
    this.getNews()
  },

  methods: {
    getNews() {
      this.$axios.get('news')
      .then(response => {
        this.news = response.data.data
        console.log(response.data.data);
      })
      .catch(e => {
        console.log(e.response.data);
      })
    },
    unpublish() {
      this.$axios.delete(`news/${this.$route.params.slug}`,{
        // console.log('suskses')
      })
    }
  }
}
</script>
<style>
.container {
  min-height: 100vh;
}

.button {
  display: flex;
  justify-content: space-between;
}
</style>