<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Wow amazing</h2>
    <p>{{ message2 }}</p>
    <h1>New Product</h1>
    <div>
      <p></p>
      Name:
      <input type="text" v-model="newProductParams.name" />
      <p></p>
      Description:
      <input type="text" v-model="newProductParams.description" />
      <p></p>
      Price:
      <input type="text" v-model="newProductParams.price" />
      <p></p>
      Image_url:
      <input type="text" v-model="newProductParams.image_url" />
      <p></p>
    </div>
    <button v-on:click="createProduct()">Create</button>
    <div v-for="product in products" :key="product.id">
      <h1>{{ product.name }}</h1>
      <h2>{{ product.price }}</h2>
      <img v-bind:src="product.image_url" alt="product.name" />
      <p></p>
      <button v-on:click="showProduct(product)">More info!</button>
      <button v-on:click="updateProduct(product)">Update!</button>
      <button v-on:click="destroyProduct(product)">Destroy!</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info!</h1>
        <p>
          Name:
          <input type="text" v-model="currentProduct.name" />
        </p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        <p>
          Price:
          <input type="text" v-model="currentProduct.price" />
        </p>
        <p>
          Image_url:
          <input type="text" v-model="currentProduct.image_url" />
        </p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to The Best Store!",
      message2: "Teeny owl feet",
      products: [],
      newProductParams: {},
      currentProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log("All products:", this.products);
      });
    },
    createProduct: function () {
      console.log("Making a product");
      axios
        .post("http://localhost:3000/products", this.newProductParams)
        .then((response) => {
          console.log("Nice", response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (product) {
      var editProductParams = product;
      axios.patch("http://localhost:3000/products/" + product.id, editProductParams).then((response) => {
        console.log("You did it!", response.data);
      });
    },
    destroyProduct: function (product) {
      axios.delete("http://localhost:3000/products/" + product.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>
