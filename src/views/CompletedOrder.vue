<template>
  <div class="order-competed">
    <Navbar />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img src="../assets/images/success.jpg" width="300" />
          <h2 class="mt-4"><strong>Success!!</strong></h2>
          <h4>
            Your Order is on process <br />
            Thanks
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "CompletedOrder",
  components: {
    Navbar,
  },
};
</script>

<style>
</style>