<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post" @submit.prevent="create()" enctype="multipart/form-data">
          <div class="row">
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_name">Name</label>
              <input id="product_name" name="name" class="form-control form-control-sm" v-model="product.name" required maxlength="50" />
              <span v-if="errors.name" class="text-danger">{{errors.name}}</span>
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_price">Price</label>
              <input id="product_price" name="price" class="form-control form-control-sm" v-model="product.price" type="number" step="0.1" required />
              <span v-if="errors.price" class="text-danger">{{errors.price}}</span>
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_brand_id">Brand</label>
              <select id="product_brand_id" name="brand_id" class="form-select form-select-sm" v-model="product.brand_id" required>
                <option v-for="brand in brands" :key="brand" :value="brand.id" :selected="product.brand_id == brand.id">{{brand.name}}</option>
              </select>
              <span v-if="errors.brand_id" class="text-danger">{{errors.brand_id}}</span>
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_image">Image</label>
              <input type="file" accept="image/*" id="product_image" name="imageFile" class="form-control form-control-sm" maxlength="50" />
              <span v-if="errors.image" class="text-danger">{{errors.image}}</span>
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/product')">Cancel</router-link>
              <button class="btn btn-sm btn-primary">Submit</button>
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
  name: 'ProductCreate',
  data() {
    return {
      product: {},
      brands: [],
      errors: {}
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
      return Service.create().then(response => {
        this.brands = response.data.brands
      }).catch(e => {
        alert(e.response.data.message)
      })
    },
    create() {
      let data = { ...this.product }
      data.imageFile = document.getElementsByName('imageFile')[0].files[0]
      data = Util.getFormData(data)
      Service.create(data).then(() => {
        this.$router.push(this.getRef('/product'))
      }).catch((e) => {
        if (e.response.data.errors) {
          this.errors = e.response.data.errors
        }
        else {
          alert(e.response.data.message)
        }
      })
    }
  }
}
</script>
