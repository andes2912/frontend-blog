<template>
  <div class="container-fluid container">
    <div class="card shadow mb-2">
      <div class="card-body">
        <h5 class="font-monospace">Category</h5>
      </div>
    </div>
    <div class="row">
      <div class="col-md-6 ">
        <div class="card shadow">
          <div class="card-body">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Category Name</th>
                  <th scope="col">Status</th>
                  <th scope="col">Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(categori, index) in categories" :key="index">
                  <th scope="row">{{categori.id}}</th>
                  <td>{{categori.name}}</td>
                  <td>{{categori.status == 1 ? 'Aktif' : 'Tidak Aktif'}}</td>
                  <td>
                    <nuxt-link :to="'/category/' + categori.slug" class="btn btn-info btn-sm text-white">Edit</nuxt-link>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card shadow mb-2">
          <div class="card-body">
            <h5 class="font-monospace">Edit Category</h5>
          </div>
        </div>
        <div class="card shadow">
          <div class="card-body">
            <form method="post" @submit.prevent="updateCategory">
              <div class="mb-3">
                <label for="category" class="form-label">Name Category</label>
                <input type="text" class="form-control" v-model="detail.name" id="category" placeholder="Name Category">
              </div>

              <div class="mb-3">
                <label for="category" class="form-label">Status</label>
                <select class="form-control" v-model="detail.status">
                  <option value="1">Aktif</option>
                  <option value="0">Tidak Aktif</option>
                </select>
              </div>
              <div class="alert alert-danger" v-for="(error, index) in errors" :key="index">
                {{ error[0] }}
              </div>
              <button type="submit" class="btn btn-primary">Update</button>
              <nuxt-link class="btn btn-warning text-white" to="/category">Cancel</nuxt-link>
            </form>
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
      categories: [],
      detail: [],
      errors: null
    }
  },

  mounted() {
    this.editCategory(),
    this. getCategory()
  },

  methods: {
    editCategory() {
      this.$axios.get(`category/${this.$route.params.slug}`)
      .then(response => {
        this.detail = response.data.data
      })
      .catch(e => {
        this.errors = e.response.data.errors
      })
    },

    updateCategory() {

      this.$axios.put(`category/${this.$route.params.slug}`, {
        name: this.detail.name,
        status: this.detail.status
      })
      .then(() => {
        this.$router.push('/category')
      })
      .catch(e => {
        this.errors = e.response.data.errors
      })
    },

    getCategory() {
      this.$axios.get('category')
      .then(response => {
        this.categories = response.data.data

      })
      .catch(error => {
        this.errors = error.response.data.errors
      })
    }
  }
}
</script>
<style>
.container {
  min-height: 100vh;
}
</style>