<template>
  <div class="container mt-4">
    <h1 class="text-center mb-4" >Shopping Cart</h1>


    <div class="container mt-4">
      <form @submit.prevent="addProduct" class="row mb-3">
        <input v-model="newProduct.name" placeholder="Product" class="form-control col" required />
        <input v-model.number="newProduct.price" type="number" placeholder="Price" class="form-control col" required />
        <input v-model.number="newProduct.quantity" type="number" placeholder="Quantity" class="form-control col" required />
        <button class="btn btn-primary col">Add</button>
      </form>
    </div>
    <br>


  <div>
    <table class="table " >
      <thead>
      <tr >
        <th >Product</th>
        <th class="text-center">Quantity</th>
        <th class="text-center">Price</th>
        <th class="text-center">Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in cartItems" :key="item">
        <td>{{ item.name }}</td>
        <td class="text-center">{{ item.quantity }}</td>
        <td class="text-center">${{ item.price }}</td>
        <td class="text-center">
          <button @click="removeProduct(item.name)" class="btn btn-danger btn-sm">Remove</button>
        </td>

      </tr>
      </tbody>
    </table>
  </div>
    <div>
      <h5 class="mt-4">Tax: ${{tax.toFixed(2) }}</h5>
      <h5 class="mt-4">Total: ${{cartTotal.toFixed(2) }}</h5>
      <h3 class="mt-4">Sum With Tax: ${{taxtotal.toFixed(2)}}</h3>
    </div>
  </div>

</template>

<!--  <div class="container mt-1 mb-1 p-0">

    <br>
    <h1 class="text-center">Shopping Cart</h1>
    <br>
    <div class="row g-0 align-items-center mb-4">
      <div class="col">
        <input type="text" class="form-control" placeholder="Product" />
      </div>
      <div class="col">
        <input type="number" class="form-control" placeholder="Price" />
      </div>
      <div class="col">
        <input type="number" class="form-control" placeholder="Quantity" />
      </div>
      <div class="col">
        <button type ="button" class="btn btn-primary w-100">Add</button>
      </div>
    </div>
    <br>
    <br>


    <div class="list-group" >
      <div class="list-group-item d-flex justify-content-between align-items-center border-0"  >
        <span>2x Orange @ $1.2</span>
        <button class="btn btn-danger border-0">Remove</button>
      </div>
      <div class="list-group-item d-flex justify-content-between align-items-center border-0">
        <span>3x Banana @ $1.2</span>
        <button class="btn btn-danger">Remove</button>
      </div>
      <div class="list-group-item d-flex justify-content-between align-items-center border-0">
        <span>1x Apple @ $1.4</span>
        <button class="btn btn-danger">Remove</button>
      </div>
    </div>

    <div>
      <h3 class="mt-4">Total: ${{ cartTotal.toFixed(2) }}</h3>
    </div>
  </div>

  <br>
  <br>

  <div class="d-grid gap-2 col-6 mx-auto">
    <button type="button" class="btn btn-outline-danger">test</button>

  </div>

</template>-->


  <script>
    import axios from "axios";

    export default {
      data: () => ({
        api: "http://localhost:8089/api/cart",
        newProduct: {name: '', price: "", quantity: "",},
        cartItems: [],
        cartTotal: 0,
        tax: 0,
        taxtotal: 0
      }),
      methods: {
        fetchCart() {
          axios.all([
            axios.get(`${this.api}/get-cart-items`).then(res => (this.cartItems = res.data)),
            axios.get(`${this.api}/calculateCartTotal`).then(res => (this.cartTotal = res.data)),
            axios.get(`${this.api}/tax`).then(res => (this.tax = res.data)),
            axios.get(`${this.api}/taxtotal`).then(res => (this.taxtotal = res.data))
          ])
          },
        removeProduct(name) {
          axios.delete(`${this.api}/remove/${name}`).then(this.fetchCart);
        },
        addProduct() {
          axios.post(`${this.api}/add-product`,this.newProduct).then(this.fetchCart);
          this.newProduct ={name:"",price: 0,quantity: 1};
        }
      },

      mounted() {
        this.fetchCart();
      },
    }
  </script>


<style>


</style>

