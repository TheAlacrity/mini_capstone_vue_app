<template>
  <div class="home">
    <h1> New Product</h1>
    <div>
      Name: <input v-model="newProductName">
    </div>

    <div>
      Price: <input v-model="newProductPrice">
    </div>

    <div>
      Description: <input v-model="newProductDescription">
    </div>

    <div>
      Image URL: <input v-model="newProductImageUrl">
    </div>

    <button v-on:click="addProduct()">Add Product</button>

    <h1>All Products</h1>

    <div v-for="product in products">
      <img v-bind:src="product.image_url" alt="">
      <h2><router-link v-bind:to="'/products/' + product.id">Name: {{ product.name }} </router-link></h2>
      <p>ID: {{ product.id }}</p>


      <div v-if="currentProduct === product">
        <p>Price: {{ product.price }}</p>
        <p>Description: {{ product.description }}</p>

        <h5>Edit Product</h5>
        <div>
          <div>
            Name: <input v-model="product.name">
          </div>
          <div>
            Price: <input v-model="product.price">
          </div>
          <div>
            Description: <input v-model="product.description">
          </div>
          <div>
            Image URL: <input v-model="product.image_url">
          </div>
        </div>
        <button v-on:click="destroyProduct(product)">Delete</button>

      </div>
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
  </div>

</template>

<style>
img {
  width: 250px;
}
</style>

<script>
  var axios = require('axios');

export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
      currentProduct: {
                      name: "",
                      price: "",
                      description: "",
                      image_url: ""
                      }
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/products").then(response => {
      this.products = response.data;
    })
  },
  methods: {
    showProduct: function(inputProduct) {
      this.currentProduct = inputProduct;
    },
    addProduct: function() {
      console.log("Add a new product");
      var params = {
                       name: this.newProductName,
                       price: this.newProductPrice,
                       description: this.newProductDescription,
                       image_url: this.newProductImageUrl
                       };
      axios.post("http://localhost:3000/api/products", params).then(response => {
         console.log("Successfully created new product", response.data);
        this.products.push(response.data);

        this.newProductName = "";
        this.newProductPrice = "";
        this.newProductDescription = "";
        this.newProductImageUrl = "";
      });
    },
    updateProduct: function(inputProduct) {
      this.currentProduct = inputProduct;
      
      var params = {
                     name: inputProduct.name,
                     price: inputProduct.price,
                     description: inputProduct.description,
                     image_url: inputProduct.image_url
                    };

      axios.patch("/api/products/" + inputProduct.id, params).then(response => {
        console.log("Success", response.data);
      });
    },
    destroyProduct: function(inputProduct) {
      axios.delete("/api/products/" + inputProduct.id).then(response => {
        console.log("Success", response.data);
        var index = this.products.indexOf(inputProuct);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>