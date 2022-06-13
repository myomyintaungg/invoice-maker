<template>
  <div class="container">
    <div class="row">
      <div class="col-8 m-auto">
        <h3 class="text-center text-primary mt-5">Invoice Maker</h3>
        <form action="" class="" @submit.prevent="saveRecord()">
          <div class="row g-1">
            <div class="col">
              <select v-model="selectedProduct" class="form-select">
                <option value="">Select product</option>
                <option
                  v-for="product in products"
                  :key="product.id"
                  :value="product.id"
                >
                  {{ product.name }} (${{ product.price }})
                </option>
              </select>
            </div>
            <div class="col">
              <input
                type="number"
                class="form-control"
                v-model="inpQuantity"
                placeholder="Quantity"
              />
            </div>
            <div class="col">
              <button class="btn btn-primary w-100 fw-bold">+</button>
            </div>
          </div>
        </form>
        <div v-if="hasRecord === false" class="mt-5">
          <h3 class="text-primary text-center">No Record Yet!!</h3>
        </div>
        <div v-else class="mt-5">
          <h3 class="text-center text-primary">Records</h3>
          <table class="table table-bordered">
            <thead>
              <tr>
                <th>#</th>
                <th>Product</th>
                <th>Price</th>
                <th>Quantity</th>
                <th>Option</th>
                <th>Cost</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(record, index) in records" :key="index">
                <td>{{}}</td>
                <td>{{ record.product.name }}</td>
                <td>{{ record.product.price }}</td>
                <td>{{ record.quantity }}</td>
                <td>
                  <button
                    class="btn btn-outline-danger btn-sm"
                    @click="del(index)"
                  >
                    Delete
                  </button>
                </td>
                <td>{{ record.cost }}</td>
              </tr>
            </tbody>
            <tfoot v-if="hasRecord === true">
              <tr>
                <td colspan="5" class="fw-bold text-center">Total Cost</td>
                <td>{{ totalCost }}</td>
              </tr>
            </tfoot>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedProduct: "",
      inpQuantity: "",
      records: [],
      products: [
        {
          id: 1,
          name: "apple",
          price: 500,
        },
        {
          id: 2,
          name: "orange",
          price: 200,
        },
        {
          id: 1,
          name: "mango",
          price: 300,
        },
      ],
    };
  },
  methods: {
    saveRecord() {
      let currentProduct = this.products.find(
        (el) => el.id === this.selectedProduct
      );
      let calcCost = currentProduct.price * this.inpQuantity;
      let record = {
        product: currentProduct,
        quantity: this.inpQuantity,
        cost: calcCost,
      };
      this.records.push(record);
      this.selectedProduct = "";
      this.inpQuantity = "";
    },
    del(index) {
      this.records = this.records.filter((el, i) => i != index);
    },
  },
  computed: {
    totalCost() {
      return this.records.reduce((p, c) => p + c.cost, 0);
    },
    hasRecord() {
      if (this.records.length === 0) {
        return false;
      }
      return true;
    },
  },
};
</script>

<style>
@import "~bootstrap/dist/css/bootstrap.min.css";
</style>
