<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post">
          <div class="row">
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="brand_id">Id</label>
              <input readonly id="brand_id" name="id" class="form-control form-control-sm" :value="brand.id" type="number" required />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="brand_name">Name</label>
              <input readonly id="brand_name" name="name" class="form-control form-control-sm" :value="brand.name" required maxlength="50" />
            </div>
            <div class="col-12">
              <h6>Brand's products</h6>
              <table class="table table-sm table-striped table-hover">
                <thead>
                  <tr>
                    <th>Product Name</th>
                    <th>Price</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="brandProduct in brandProducts" :key="brandProduct">
                    <td>{{brandProduct.name}}</td>
                    <td class="text-end">{{brandProduct.price}}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/brand')">Back</router-link>
              <router-link class="btn btn-sm btn-primary" :to="`/brand/edit/${brand.id}?ref=${encodeURIComponent(getRef('/brand'))}`">Edit</router-link>
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
  name: 'BrandDetail',
  data() {
    return {
      brand: {},
      brandProducts: [],
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
      return Service.get(this.$route.params.id).then(response => {
        this.brand = response.data.brand
        this.brandProducts = response.data.brandProducts
      }).catch(e => {
        alert(e.response.data.message)
      })
    }
  }
}
</script>
