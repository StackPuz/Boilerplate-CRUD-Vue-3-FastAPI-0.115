<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post">
          <div class="row">
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_header_id">Id</label>
              <input readonly id="order_header_id" name="id" class="form-control form-control-sm" :value="orderHeader.id" type="number" required />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="customer_name">Customer</label>
              <input readonly id="customer_name" name="customer_name" class="form-control form-control-sm" :value="orderHeader.customer_name" maxlength="50" />
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_header_order_date">Order Date</label>
              <input readonly id="order_header_order_date" name="order_date" class="form-control form-control-sm" :value="orderHeader.order_date" data-type="date" autocomplete="off" required />
            </div>
            <div class="col-12">
              <table class="table table-sm table-striped table-hover">
                <thead>
                  <tr>
                    <th>No</th>
                    <th>Product</th>
                    <th>Qty</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="orderHeaderOrderDetail in orderHeaderOrderDetails" :key="orderHeaderOrderDetail">
                    <td class="text-center">{{orderHeaderOrderDetail.no}}</td>
                    <td>{{orderHeaderOrderDetail.product_name}}</td>
                    <td class="text-end">{{orderHeaderOrderDetail.qty}}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/orderHeader')">Back</router-link>
              <router-link class="btn btn-sm btn-primary" :to="`/orderHeader/edit/${orderHeader.id}?ref=${encodeURIComponent(getRef('/orderHeader'))}`">Edit</router-link>
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
  name: 'OrderHeaderDetail',
  data() {
    return {
      orderHeader: {},
      orderHeaderOrderDetails: [],
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
        this.orderHeader = response.data.orderHeader
        this.orderHeaderOrderDetails = response.data.orderHeaderOrderDetails
      }).catch(e => {
        alert(e.response.data.message)
      })
    }
  }
}
</script>
