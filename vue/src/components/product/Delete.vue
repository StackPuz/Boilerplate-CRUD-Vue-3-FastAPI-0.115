<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post" @submit.prevent="this.delete()">
          <div class="row">
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_id">Id</label>
              <input readonly id="product_id" name="id" class="form-control form-control-sm" :value="product.id" type="number" required />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_name">Name</label>
              <input readonly id="product_name" name="name" class="form-control form-control-sm" :value="product.name" required maxlength="50" />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_price">Price</label>
              <input readonly id="product_price" name="price" class="form-control form-control-sm" :value="product.price" type="number" step="0.1" required />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="brand_name">Brand</label>
              <input readonly id="brand_name" name="brand_name" class="form-control form-control-sm" :value="product.brand_name" maxlength="50" />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="user_account_name">Create User</label>
              <input readonly id="user_account_name" name="user_account_name" class="form-control form-control-sm" :value="product.user_account_name" maxlength="50" />
            </div>
            <div class="mb-3 col-md-6 col-lg-4"><label class="form-label">Image</label>
              <div><a :href="`http://localhost:8000/uploads/products/${product.image}`" target="_blank" :title="`${product.image}`"><img class="img-item" :src="`http://localhost:8000/uploads/products/${product.image}`" /></a></div>
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/product')">Cancel</router-link>
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
  name: 'ProductDelete',
  data() {
    return {
      product: {}
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
        this.product = response.data.product
      }).catch(e => {
        alert(e.response.data.message)
      })
    },
    delete() {
      Service.delete(this.$route.params.id, this.product).then(() => {
        this.$router.push(this.getRef('/product'))
      }).catch((e) => {
        alert(e.response.data.message)
      })
    }
  }
}
</script>
