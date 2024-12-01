<script>
import axios from "axios";

export default {
  data() {
    return {
      customerId: "",
      invoiceNumber: "",
      order: {}
    };
  },
  methods: {
    fetchOrder() {
      if (this.customerId && this.invoiceNumber) {
        axios.get(`http://127.0.0.1:8000/api/orders/${this.customerId}/${this.invoiceNumber}`)
          .then(response => {
            this.order = response.data;
          })
          .catch(error => {
            console.error("Error fetching order:", error);
            this.order = { error: "Order not found. Please check the inputs and try again." };
          });
      }
    }
  }
}
</script>

<template>
  <div class="container mt-5">
    <h2 class="text-center">Order Information</h2>
    <div class="row justify-content-center">
      <div class="col-md-6">
        <form id="searchForm" class="form-inline" @submit.prevent="fetchOrder">
          <div class="mb-3">
            <label for="customerIdInput" class="form-label">Customer ID:</label>
            <input type="text" v-model="customerId" class="form-control" id="customerIdInput"
              placeholder="Enter Customer ID" required>
          </div>
          <div class="mb-3">
            <label for="invoiceNumberInput" class="form-label">Invoice Number:</label>
            <input type="text" v-model="invoiceNumber" class="form-control" id="invoiceNumberInput"
              placeholder="Enter Invoice Number" required>
          </div>
          <button type="button" class="btn btn-primary" @click="fetchOrder">Search</button>
        </form>
      </div>
    </div>
    <div class="row justify-content-center mt-4" v-if="order">
      <div class="col-md-6">
        <div id="result" class="alert alert-info">
          <p v-if="order.customer_id">Customer ID: {{ order.customer_id }}</p>
          <p v-if="order.invoice_number">Invoice Number: {{ order.invoice_number }}</p>
          <p v-if="order.order_status">Order Status: {{ order.order_status }}</p>
          <p v-if="order.error" class="text-danger">{{ order.error }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
