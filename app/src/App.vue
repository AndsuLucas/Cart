<template>
  <div id="app">
    <products @productAdded="addProductOnCart"></products>
    <hr>
    <cart :products="productsOnCart"
      @decreaseStock="decreaseProductStock" @increaseStock="increaseProductStock"
    ></cart>
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
      const productOnCart = this.getProductOnCart(product);
      this.decreaseProductStock(product, 1);
      if (productOnCart != undefined) {
        this.increaseProductQuantity(productOnCart);
        return;
      }

      product.quantity = 1;
      this.productsOnCart.push(product);
    },

    decreaseProductStock(product, quantityToDrecrease) {
      const newStock = product.stock - quantityToDrecrease;
      this.$set(product, 'stock', newStock);
    },

    increaseProductStock(product, quantityToIncrease) {
       const newStock = product.stock + quantityToIncrease;
      this.$set(product, 'stock', newStock);
    },

    increaseProductQuantity(product) {
      const index = product.index;
      if (index === undefined) {
        throw Error('Product without index');
      }

      delete product.index;
      product.quantity ++;
      this.$set(this.productsOnCart, index, product);
    },
  
    getProductOnCart(product) {
      const productOnCart = this.productsOnCart.find((productOnCart, index) => {
        productOnCart.index = index;
        return product.id === productOnCart.id
      });

      return productOnCart;
    }
  }
}
</script>

<style>
</style>
