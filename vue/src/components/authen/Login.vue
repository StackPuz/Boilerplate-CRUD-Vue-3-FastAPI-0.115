<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <div class="center-container">
          <div class="d-flex justify-content-center">
            <div class="card card-width p-0">
              <div class="card-header">
                <h3>Login</h3>
              </div>
              <div class="card-body">
                <i class="login fa fa-user-circle"></i>
                <form method="post" @submit.prevent="login()">
                  <div class="row">
                    <div class="mb-3 col-12">
                      <label class="form-label" for="user_account_name">User Name</label>
                      <input id="user_account_name" name="name" class="form-control form-control-sm" v-model="user.name" required maxlength="50" />
                    </div>
                    <div class="mb-3 col-12">
                      <label class="form-label" for="user_account_password">Password</label>
                      <input id="user_account_password" name="password" class="form-control form-control-sm" v-model="user.password" type="password" required maxlength="100" />
                    </div>
                    <div class="col-12">
                      <button class="btn btn-sm btn-secondary w-100">Login</button>
                      <router-link to="/resetPassword">Forgot Password?</router-link>
                    </div>
                  </div>
                </form>
                <span v-if="error" class="text-danger">{{error.message}}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import http from '../../http'
  
  export default {
    name: 'Login',
    data() {
      return {
        user: {},
        error: null
      }
    },
    beforeMount() {
      if (this.$root.user) {
        this.$router.push(this.$router.options.history.redirect || '/home')
      }
    },
    methods: {
      login() {
        http.post('/login', this.user).then(response => {
          this.$root.user = response.data.user
          localStorage.setItem('fast_token', response.data.token)
        }).catch((e) => {
          this.error = e.response.data
        })
      }
    }
  }
</script>