<template>
  <div id="app">
    <products @productAdded="addProductOnCart"></products>
    <hr>
    <cart :products="productsOnCart"></cart>
  </div>
</template>

<script>
import cart from '@/components/Cart/Cart';
import products from '@/components/Products/Products';

export default {
  name: 'App',
  components: {cart, products},

  data() {
    return {
      productsOnCart: [],
    }
  },

  methods: {
    addProductOnCart(product) {
      this.productsOnCart.push(product);
    }
  }
}
</script>

<style>
</style>
