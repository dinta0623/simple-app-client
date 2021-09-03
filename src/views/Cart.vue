<template>
  <div class="cart">
    <Navbar :updateCart="carts" />
    <div class="container">
      <!-- breadcrumb -->
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Cart</li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>
            Your
            <strong>Cart</strong>
          </h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Product</th>
                  <th scope="col">Description</th>
                  <th scope="col">Quantity</th>
                  <th scope="col" class="text-right">Price</th>
                  <th scope="col" class="text-right">Total Price</th>
                  <th scope="col" class="text-center">Remove</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(cart, index) in carts" :key="cart._id">
                  <th>{{ index + 1 }}</th>
                  <td>
                    <strong>{{ cart.products.name }}</strong>
                  </td>
                  <td>
                    {{ cart.desc ? cart.desc : "-" }}
                  </td>
                  <td>{{ cart.qty }}</td>
                  <td align="right">$ {{ cart.products.price }}</td>
                  <td align="right">
                    <strong>$ {{ cart.products.price * cart.qty }}</strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash
                      class="point"
                      @click="removeCart(cart._id)"
                    ></b-icon-trash>
                  </td>
                </tr>

                <tr>
                  <td colspan="5" align="right">
                    <strong>Total :</strong>
                  </td>
                  <td align="right">
                    <strong>$ {{ hitungTotal }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- Checkout -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <button type="submit" class="btn btn-success" @click="checkout">
              <b-icon-cart></b-icon-cart> Checkout
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
  name: "Cart",
  components: {
    Navbar,
  },
  data() {
    return {
      carts: [],
      pesanan: {},
    };
  },
  methods: {
    setCarts(data) {
      this.carts = data;
    },
    removeCart(id) {
      axios
        .delete("https://simple-cart-apis.herokuapp.com/api/carts/" + id)
        .then(() => {
          this.$toast.error("Successfully removed from the cart", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });

          // Update Data keranjang
          axios
            .get("https://simple-cart-apis.herokuapp.com/api/carts")
            .then((response) => this.setCarts(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
    checkout() {
      this.pesanan.carts = this.carts;
      axios
        .post("https://simple-cart-apis.herokuapp.com/api/order", this.pesanan)
        .then(() => {
          // Hapus Semua Keranjang
          this.carts.map(function (item) {
            console.log(item._id);
            return axios
              .delete(
                "https://simple-cart-apis.herokuapp.com/api/carts/" + item._id
              )
              .catch((error) => console.log(error));
          });

          this.$router.push({ path: "/order-complete" });
          this.$toast.success("Successfully Ordered", {
            type: "success",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
        })
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    axios
      .get("https://simple-cart-apis.herokuapp.com/api/carts")
      .then((response) => this.setCarts(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    hitungTotal() {
      return this.carts.reduce(function (items, data) {
        return items + data.products.price * data.qty;
      }, 0);
    },
  },
};
</script>

<style scoped>
.point {
  cursor: pointer;
}
</style>