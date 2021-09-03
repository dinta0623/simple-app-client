<template>
  <div>
    <b-navbar toggleable="lg" type="light">
      <div class="container">
        <b-navbar-brand href="#">Simple-app</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link class="nav-link" to="/">Home</router-link>
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/cart">
                Cart
                <b-icon-bag></b-icon-bag>
                <span class="badge badge-dark ml-2">{{ qty.length }}</span>
              </router-link>
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  data() {
    return {
      qty: [],
    };
  },
  props: ["updateCart"],
  methods: {
    setQty(data) {
      this.qty = data;
    },
  },
  mounted() {
    axios
      .get("https://simple-cart-apis.herokuapp.com/api/carts")
      .then((response) => this.setQty(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style></style>
