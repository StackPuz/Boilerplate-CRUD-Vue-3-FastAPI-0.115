<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post" @submit.prevent="this.delete()">
          <div class="row">
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_detail_order_id">Order Id</label>
              <input readonly id="order_detail_order_id" name="order_id" class="form-control form-control-sm" :value="orderDetail.order_id" type="number" required />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_detail_no">No</label>
              <input readonly id="order_detail_no" name="no" class="form-control form-control-sm" :value="orderDetail.no" type="number" required />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="product_name">Product</label>
              <input readonly id="product_name" name="product_name" class="form-control form-control-sm" :value="orderDetail.product_name" maxlength="50" />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_detail_qty">Qty</label>
              <input readonly id="order_detail_qty" name="qty" class="form-control form-control-sm" :value="orderDetail.qty" type="number" required />
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/orderDetail')">Cancel</router-link>
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
  name: 'OrderDetailDelete',
  data() {
    return {
      orderDetail: {}
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
      return Service.delete(this.$route.params.orderId, this.$route.params.no).then(response => {
        this.orderDetail = response.data.orderDetail
      }).catch(e => {
        alert(e.response.data.message)
      })
    },
    delete() {
      Service.delete(this.$route.params.orderId, this.$route.params.no, this.orderDetail).then(() => {
        this.$router.push(this.getRef('/orderDetail'))
      }).catch((e) => {
        alert(e.response.data.message)
      })
    }
  }
}
</script>
