<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post" @submit.prevent="this.delete()">
          <div class="row">
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="user_account_id">Id</label>
              <input readonly id="user_account_id" name="id" class="form-control form-control-sm" :value="userAccount.id" type="number" required />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="user_account_name">Name</label>
              <input readonly id="user_account_name" name="name" class="form-control form-control-sm" :value="userAccount.name" required maxlength="50" />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="user_account_email">Email</label>
              <input readonly id="user_account_email" name="email" class="form-control form-control-sm" :value="userAccount.email" type="email" required maxlength="50" />
            </div>
            <div class="form-check col-md-6 col-lg-4">
              <input readonly id="user_account_active" name="active" class="form-check-input" type="checkbox" :value="userAccount.active" :checked="userAccount.active" />
              <label class="form-check-label" for="user_account_active">Active</label>
            </div>
            <div class="col-12">
              <h6>Roles</h6>
              <table class="table table-sm table-striped table-hover">
                <thead>
                  <tr>
                    <th>Role Name</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="userAccountUserRole in userAccountUserRoles" :key="userAccountUserRole">
                    <td>{{userAccountUserRole.role_name}}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/userAccount')">Cancel</router-link>
              <button class="btn btn-sm btn-danger">Delete</button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import Service from './Service'
import Util from"../../util"

export default {
  name: 'UserAccountDelete',
  data() {
    return {
      userAccount: {},
      userAccountUserRoles: [],
    }
  },
  mounted() {
    this.get().finally(() => {
      this.initView(true)
    })
  },
  methods: {
    ...Util,
    get() {
      return Service.delete(this.$route.params.id).then(response => {
        this.userAccount = response.data.userAccount
        this.userAccountUserRoles = response.data.userAccountUserRoles
      }).catch(e => {
        alert(e.response.data.message)
      })
    },
    delete() {
      Service.delete(this.$route.params.id, this.userAccount).then(() => {
        this.$router.push(this.getRef('/userAccount'))
      }).catch((e) => {
        alert(e.response.data.message)
      })
    }
  }
}
</script>
