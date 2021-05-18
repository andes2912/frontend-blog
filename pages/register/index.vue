<template>
  <section class="h-100">
		<div class="container h-100">
			<div class="row justify-content-sm-center h-100">
				<div class="col-xxl-4 col-xl-5 col-lg-5 col-md-7 col-sm-9">
					<div class="card shadow-lg">
						<div class="card-body p-5">
              <div class="alert alert-danger" v-for="(error, index) in errors" :key="index">
              {{ error[0] }}
              </div>
							<h1 class="fs-4 card-title fw-bold mb-4 text-center font-monospace">Register</h1>
							<form method="POST" class="needs-validation" novalidate="" autocomplete="off" @submit.prevent="register">
								<div class="mb-3">
									<label class="mb-2 text-muted" for="name">Name</label>
									<input id="name" type="text" class="form-control" v-model="name" value="" required autofocus>
								</div>

								<div class="mb-3">
									<label class="mb-2 text-muted" for="email">E-Mail Address</label>
									<input id="email" type="email" class="form-control" v-model="email" required>
								</div>

                <div class="mb-3">
                  <label class="mb-2 text-muted" for="gender">Gender</label>
                  <select v-model="gender" class="form-control">
                    <option value="1">Laki-Laki</option>
                    <option value="0">Perempuan</option>
                  </select>
                </div>

								<div class="mb-3">
									<label class="mb-2 text-muted" for="password">Password</label>
									<input id="password" type="password" class="form-control" v-model="password" required>
								</div>

                <div class="mb-3">
									<label class="mb-2 text-muted" for="password">Password Confirmation</label>
									<input id="password" type="password" class="form-control" v-model="password_confirmation" required>

								</div>
								<div class="align-items-center d-flex">
                  <div>
										<nuxt-link to="/" style="text-decoration:none; color:black">Back to Homepage</nuxt-link>
									</div>
									<button type="submit" class="btn btn-primary ms-auto">
										Register
									</button>
								</div>
							</form>
						</div>
						<div class="card-footer py-3 border-0">
							<div class="text-center">
								Already have an account? <nuxt-link to="login" class="text-dark">Login</nuxt-link>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>
</template>

<script>
export default {
 layout: 'auth',
 middleware: 'guest',
  data() {
    return {
      name: '',
      email: '',
      gender: '',
      password: '',
      password_confirmation: '',
      errors: null
    }
  },
  methods: {
    async register() {
      try {
        await this.$axios.post('register', {
          name: this.name,
          email: this.email,
          gender: this.gender,
          password: this.password,
          password_confirmation: this.password_confirmation
        })

      this.$router.push('/login')

      } catch (e) {
        this.errors = e.response.data.errors
      }
    }
  }
}
</script>
<style>
.container {
  min-height: 100vh;
}
</style>