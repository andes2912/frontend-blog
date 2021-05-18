<template>
  <div class="container container-fluid">
    <div class="card shadow mb-2">
      <div class="card-body">
        <h5 class="font-monospace">Form News</h5>
      </div>
    </div>
    <div class="card shadow">
      <div class="card-body">
        <form method="post" @submit.prevent="news">
          <div class="row">
            <div class="alert alert-danger" v-for="(error, index) in errors" :key="index">
              {{ error[0] }}
            </div>
            <div class="col-8">
              <div class="mb-3">
                <label for="title" class="form-label">Title</label>
                <input type="text" class="form-control" v-model="title" id="title" placeholder="Enter Ttile">
              </div>
              <div class="mb-3">
                <label for="exampleFormControlTextarea1" class="form-label">Content</label>
                <no-ssr>
                  <quill-editor v-model='content' ref='textEditor' :options='editorOption'></quill-editor>
                </no-ssr>
              </div>
            </div>
            <div class="col-3">
              <div class="mb-3">
                <label for="category" class="form-label">Category</label>
                <select class="form-select" v-model="category_id">
                  <option v-for="(categori, index) in category" :key="index" :value="categori.id">{{categori.name}}</option>
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
  middleware: 'auth',
  data() {
    return {
      title: '',
      content: '',
      category_id: '',
      category: [],
      errors: null,
      editorOption: {
        placeholder: 'What’s on your mind?',
        theme: 'snow'
      }
    }
  },

  mounted() {
    this.getCategory()
  },

  methods: {
    async news() {
      try {
        await this.$axios.post('news', {
          title: this.title,
          content: this.content,
          category_id: this.category_id,
        })
        .then(() => {
          this.title = '',
          this.content = '',
          this.category_id = '',
          this.$router.push('/news')
        })
      } catch (e) {
        this.errors = e.response.data.errors
      }
    }
    ,
    getCategory() {
      this.$axios.get('category')
      .then(response => {
        this.category = response.data.data
      })
      .catch(error => {
        console.log(error.response.data);
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