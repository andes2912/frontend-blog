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
                  <nuxt-link :to="'category/' + categori.slug" class="btn btn-info btn-sm text-white">Edit</nuxt-link>
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
          <h5 class="font-monospace">Add Category</h5>
        </div>
      </div>
      <div class="card shadow">
        <div class="card-body">
          <form method="post" @submit.prevent="category">
            <div class="mb-3">
              <label for="category" class="form-label">Name Category</label>
              <input type="text" class="form-control" v-model="name" id="category" placeholder="Name Category">
            </div>
            <div class="alert alert-danger" v-for="(error, index) in errors" :key="index">
              {{ error[0] }}
            </div>
            <button type="submit" class="btn btn-primary">Simpan</button>
            <button type="reset" class="btn btn-warning text-white">Cancel</button>
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
      name: '',
      errors: null,
      categories: []
    }
  },

  mounted() {
    this.getCategory()
  },

  methods: {
    async category() {
      try {
        await this.$axios.post('category', {
          name: this.name
        })
        .then(() => {
         this.name = ''

          this.$router.push('/category')
          this.getCategory()
        })

      } catch (e) {
        this.errors = e.response.data.errors
      }
    },

    getCategory() {
      this.$axios.get('category')
      .then(response => {
        this.categories = response.data.data
      })
      .catch(error => {
        console.log(error.response.data);
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