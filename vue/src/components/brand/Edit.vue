<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post" @submit.prevent="edit()">
          <div class="row">
            <input type="hidden" id="brand_id" name="id" v-model="brand.id" />
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="brand_name">Name</label>
              <input id="brand_name" name="name" class="form-control form-control-sm" v-model="brand.name" required maxlength="50" />
              <span v-if="errors.name" class="text-danger">{{errors.name}}</span>
            </div>
            <div class="col-12">
              <h6>Brand's products</h6>
              <table class="table table-sm table-striped table-hover">
                <thead>
                  <tr>
                    <th>Product Name</th>
                    <th>Price</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="brandProduct in brandProducts" :key="brandProduct">
                    <td>{{brandProduct.name}}</td>
                    <td class="text-end">{{brandProduct.price}}</td>
                    <td class="text-center">
                      <router-link class="btn btn-sm btn-secondary" :to="`/product/${brandProduct.id}`" title="View"><i class="fa fa-eye"></i></router-link>
                      <router-link class="btn btn-sm btn-primary" :to="`/product/edit/${brandProduct.id}`" title="Edit"><i class="fa fa-pencil"></i></router-link>
                      <router-link class="btn btn-sm btn-danger" :to="`/product/delete/${brandProduct.id}`" title="Delete"><i class="fa fa-times"></i></router-link>
                    </td>
                  </tr>
                </tbody>
              </table>
              <router-link class="btn btn-sm btn-primary" :to="`/product/create?product_brand_id=${brand.id}`">Add</router-link>
              <hr />
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/brand')">Cancel</router-link>
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
  name: 'BrandEdit',
  data() {
    return {
      brand: {},
      brandProducts: [],
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
      return Service.edit(this.$route.params.id).then(response => {
        this.brand = response.data.brand
        this.brandProducts = response.data.brandProducts
      }).catch(e => {
        alert(e.response.data.message)
      })
    },
    edit() {
      Service.edit(this.$route.params.id, this.brand).then(() => {
        this.$router.push(this.getRef('/brand'))
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
