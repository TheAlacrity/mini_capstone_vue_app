<template>
  <div class="products-show">
    <div class="container">
      <div class="row">
        <div class="col-sm-6">
          <h2>{{ product.name }}</h2>
          <p>Price: {{ product.price }}</p>
          <p>Description: {{ product.description }}</p>  
        

 
          <router-link class="btn btn-warning m-1" v-bind:to=" '/products/' + product.id + '/edit' ">Edit</router-link>

          <div>
            <button class="btn btn-my-color m-1" v-on:click="destroyProduct()">Delete</button>
            <h2><router-link v-bind:to="'/products/' + product.id + '/edit'">Edit</router-link></h2>
          </div>
        </div>
        <div class="col-sm-6">
          <img v-bind:src="product.image_url" class="img-fluid show-product-img">
        </div>  
      </div> <!-- end of .row -->
    </div> <!-- end of .container -->
  </div> <!-- end of .products-show -->
</template>

<style>
img.show-product-img {
  width: 100%;
}
.btn-my-color {
  background-color: deeppink;
}
</style>

<script>
  var axios = require('axios')
  export default {
    data: function() {
      return {
        product: {

                  }
      };
    },
    created: function() {
      axios.get("/api/products/" + this.$route.params.id ).then(response => {
        this.product = response.data;
      });
    },
    methods: {
      destroyProduct: function() {
        axios.delete("/api/products/" + this.product.id).then(response => {
          console.log("Success", response.data);
          this.$router.push("/");
          var index = this.products.indexOf(this.product);
          this.products.splice(index, 1);
        });
      }
    }
  };
</script>