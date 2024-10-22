<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form method="post" @submit.prevent="edit()">
          <div class="row">
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_header_id">Id</label>
              <input readonly id="order_header_id" name="id" class="form-control form-control-sm" v-model="orderHeader.id" type="number" required />
              <span v-if="errors.id" class="text-danger">{{errors.id}}</span>
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_header_customer_id">Customer</label>
              <select id="order_header_customer_id" name="customer_id" class="form-select form-select-sm" v-model="orderHeader.customer_id" required>
                <option v-for="customer in customers" :key="customer" :value="customer.id" :selected="orderHeader.customer_id == customer.id">{{customer.name}}</option>
              </select>
              <span v-if="errors.customer_id" class="text-danger">{{errors.customer_id}}</span>
            </div>
            <div class="mb-3 col-md-6 col-lg-4">
              <label class="form-label" for="order_header_order_date">Order Date</label>
              <input id="order_header_order_date" name="order_date" class="form-control form-control-sm" v-model="orderHeader.order_date" data-type="date" autocomplete="off" required />
              <span v-if="errors.order_date" class="text-danger">{{errors.order_date}}</span>
            </div>
            <div class="col-12">
              <table class="table table-sm table-striped table-hover">
                <thead>
                  <tr>
                    <th>No</th>
                    <th>Product</th>
                    <th>Qty</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="orderHeaderOrderDetail in orderHeaderOrderDetails" :key="orderHeaderOrderDetail">
                    <td class="text-center">{{orderHeaderOrderDetail.no}}</td>
                    <td>{{orderHeaderOrderDetail.product_name}}</td>
                    <td class="text-end">{{orderHeaderOrderDetail.qty}}</td>
                    <td class="text-center">
                      <router-link class="btn btn-sm btn-primary" :to="`/orderDetail/edit/${orderHeaderOrderDetail.order_id}/${orderHeaderOrderDetail.no}`" title="Edit"><i class="fa fa-pencil"></i></router-link>
                      <router-link class="btn btn-sm btn-danger" :to="`/orderDetail/delete/${orderHeaderOrderDetail.order_id}/${orderHeaderOrderDetail.no}`" title="Delete"><i class="fa fa-times"></i></router-link>
                    </td>
                  </tr>
                </tbody>
              </table>
              <router-link class="btn btn-sm btn-primary" :to="`/orderDetail/create?order_detail_order_id=${orderHeader.id}`">Add</router-link>
              <hr />
            </div>
            <div class="col-12">
              <router-link class="btn btn-sm btn-secondary" :to="getRef('/orderHeader')">Cancel</router-link>
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
  name: 'OrderHeaderEdit',
  data() {
    return {
      orderHeader: {},
      orderHeaderOrderDetails: [],
      customers: [],
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
        this.orderHeader = response.data.orderHeader
        this.orderHeaderOrderDetails = response.data.orderHeaderOrderDetails
        this.customers = response.data.customers
      }).catch(e => {
        alert(e.response.data.message)
      })
    },
    edit() {
      Service.edit(this.$route.params.id, this.orderHeader).then(() => {
        this.$router.push(this.getRef('/orderHeader'))
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
