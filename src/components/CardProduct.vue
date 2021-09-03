<template>
  <div class="card shadow card-product">
    <img :src="product.pict" class="card-img-top" alt="..." />
    <div class="card-body">
      <h5 class="card-title">{{ product.name }}</h5>
      <p class="card-text">Price : ${{ product.price }}</p>
      <router-link class="btn btn-dark" :to="'/product/' + product._id"
        ><b-icon-cart></b-icon-cart> Order</router-link
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["product"],
};
</script>

<style scoped>
.card {
  width: 18rem;
}
</style>