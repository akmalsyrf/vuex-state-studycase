<template>
  <nav class="navbar navbar-light bg-light">
    <div class="container-fluid">
      <span class="navbar-brand mb-0 h1">Navbar</span>
      <span class="navbar-brand mb-0 h1">Cart ({{ totalQty }})</span>
    </div>
  </nav>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "Header",
  computed: {
    ...mapGetters(["cart"]),
    totalQty() {
      return this.cart.reduce((total, item) => {
        return total + item.qty;
      }, 0);
    },
  },
};
</script>

<style>
nav {
  margin-bottom: 50px;
}
</style>
