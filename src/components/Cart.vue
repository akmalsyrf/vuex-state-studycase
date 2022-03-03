<template>
  <div class="">
    <div v-if="!cart.length" class="alert alert-secondary col-12" role="alert">No product in cart!</div>
    <div v-if="orderPlaced" class="alert alert-success row" role="alert">
      <span class="col-11"> Order successfully placed! </span>
      <button @click="() => (orderPlaced = false)" type="button" class="btn-close col-1" data-bs-dismiss="modal" aria-label="Close"></button>
    </div>
    <ul class="list-group">
      <li class="list-group-item" v-for="item in cart" :key="item.id">
        <div class="card" style="width: 23rem">
          <div class="d-flex">
            <div class="col-6">
              <img :src="item.imgUrl" width="80px" class="card-img-top" :alt="item.title" />
            </div>
            <div class="card-body">
              <h5 class="card-title">{{ item.title }}</h5>
              <button @click="reduceQty(item.id)" class="btn btn-qty btn-sm btn-secondary">-</button>
              x {{ item.qty }}
              <button @click="addQty(item.id)" class="btn btn-qty btn-sm btn-secondary">+</button>
            </div>
            <button @click="removeItem(item.id)" type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
        </div>
      </li>
    </ul>
    <button v-if="cart.length" :disabled="isProcessing" @click="placeOrder" class="btn btn-block btn-success mt-3 col-12">
      <span v-if="!isProcessing">Checkout ($ {{ totalPrice }})</span>
      <div v-else class="spinner-border" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </button>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
export default {
  name: "Cart",
  data() {
    return {
      isProcessing: false,
      orderPlaced: false,
    };
  },
  computed: {
    ...mapGetters(["cart"]),
    totalPrice() {
      return this.cart.reduce((total, item) => {
        return total + item.price * item.qty;
      }, 0);
    },
  },
  methods: {
    ...mapActions(["addQty", "reduceQty", "removeItem", "emptyCart"]),
    placeOrder() {
      this.isProcessing = true;
      setTimeout(() => {
        this.isProcessing = false;
        this.orderPlaced = true;
        this.emptyCart();
      }, 1000);
    },
  },
};
</script>

<style>
.card {
  padding: 20px;
}
.btn-qty {
  border-radius: 50%;
  width: 30px;
}
</style>
