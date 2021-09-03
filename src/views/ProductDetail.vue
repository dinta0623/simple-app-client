<template>
  <div class="product-detail">
    <Navbar />
    <div class="container">
      <!-- breadcrumb -->
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Product
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col-md-6">
          <img :src="product.pict" class="img-fluid shadow" />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.name }}</strong>
          </h2>
          <p>{{ product.desc }}</p>
          <hr />
          <h4>
            Price:
            <strong>$ {{ product.price }}</strong>
          </h4>
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="qty">Quantity</label>
              <input type="number" class="form-control" v-model="pesanan.qty" />
            </div>
            <div class="form-group">
              <label for="keterangan">Description</label>
              <textarea
                v-model="pesanan.desc"
                class="form-control"
                placeholder="Keterangan..."
              ></textarea>
            </div>

            <button type="submit" class="btn btn-dark" @click="order">
              <b-icon-cart></b-icon-cart> Add To Cart
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "ProductDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesanan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    order() {
      if (this.pesanan.qty) {
        this.pesanan.products = this.product;
        axios
          .post(
            "https://simple-cart-apis.herokuapp.com/api/carts",
            this.pesanan
          )
          .then(() => {
            this.$router.push({ path: "/" });
            this.$toast.success("Added to cart", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Please fill the quantity", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get(
        "https://simple-cart-apis.herokuapp.com/api/products/" +
          this.$route.params.id
      )
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>