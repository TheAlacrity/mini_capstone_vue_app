<template>
  <div class="products-edit">
   <h1> Edit Product</h1>

  <ul>
    <li v-for="error in errors">
      {{ error }}
    </li>
  </ul>

   <form v-on:submit.prevent="submit()">
     <div>
      Name: <input v-model="product.name">
     </div>

     <div>
      Name: <input v-model="product.price">
       
     </div>

     <div>
      Description: <input v-model="product.description">
       
     </div>

     <div>
       Image URL: <input v-model="product.image_url">
     </div>

     <input type="submit" value="Submit Update">
     
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
      product: {

      },    
      errors: []
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id ).then(response => {
      this.product = response.data;
    });
  },
  methods: {
    submit: function() {
      
      var params = {
                     name: this.product.name,
                     price: this.product.price,
                     description: this.product.description,
                     image_url: this.product.image_url
                    };

      axios.patch("/api/products/" + this.product.id, params).then(response => {
        console.log("Success", response.data);
        this.$router.push("/products/" + this.$route.params.id);
      });
    }
  }
};
</script>