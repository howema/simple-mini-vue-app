<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Wow amazing</h2>
    <p>{{ message2 }}</p>
    <h1>New Product</h1>
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
      Description:
      <input type="text" v-model="newProductParams.description" />
      Price:
      <input type="text" v-model="newProductParams.price" />
      Image_url:
      <input type="text" v-model="newProductParams.image_url" />
    </div>
    <button v-on:click="createProduct()">Create</button>
    <div v-for="product in products" :key="product.id">
      <h1>{{ product.name }}</h1>
      <h2>{{ product.price }}</h2>
      <img v-bind:src="product.image_url" alt="product.name" />
      <button v-on:click="showProduct(product)">More info!</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info!</h1>
        <p>Name: ...</p>
        <p>Description: ...</p>
        <p>Price: ...</p>
        <p>Image_url: ...</p>
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
      document.querySelector("#product-details").showModal();
    },
  },
};
</script>
