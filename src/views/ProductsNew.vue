<template>
  <div class="products-new">
   <h1> New Product</h1>

  <ul>
    <li v-for="error in errors">
      {{ error }}
    </li>
  </ul>

   <form v-on:submit.prevent="submit()">
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

     <input type="submit" value="Create Product">
     
   </form>

  </div>
</template>

<style>
</style>

<script>
  var axios = require('axios');
export default {
  data: function() {
    return {
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    submit: function() {
      console.log("Add a new product");
      var params = {
                       name: this.newProductName,
                       price: this.newProductPrice,
                       description: this.newProductDescription,
                       image_url: this.newProductImageUrl
                       };
      axios.post("http://localhost:3000/api/products", params).then(response => {
        this.$router.push("/");
      }).catch(error => {
        this.errors = error.response.data.errors
      });
    }
  }
};
</script>