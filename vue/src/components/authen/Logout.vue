<template>
  <span>Logout...</span>
</template>

<script>
import http from '../../http'

export default {
  name: 'Logout',
  mounted() {
    if (localStorage.getItem('fast_token')) {
      this.logout()
    }
    else {
      this.login()
    }
  },
  methods: {
    logout() {
      http.get('/logout').finally(() => {
        localStorage.removeItem('fast_token')
        this.login()
      })
    },
    login() {
      this.$router.push('login')
      this.$root.user = null
    }
  }
}
</script>