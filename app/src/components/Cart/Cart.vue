<template>
  <div class="main">
    <div class="cartTitle">
      <p>CART</p>
    </div>
    <div v-if="!hasProductsOnCart">
      <p class="cartMessage"><strong>A Empty Cart :(</strong></p>
    </div>
    <table v-else>
      <thead>
        <tr>
          <th>ID</th>
          <th>NAME</th>
          <th>PRICE</th>
          <th>IMAGE</th>
          <th>QUANTITY</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td>
            {{ product.id }}
          </td>
          <td>
            {{ product.name }}
          </td>
          <td>
            {{ product.price }}
          </td>
          <td>
            <img :src="product.img" alt="">
          </td>
          <td>
            <input type="number" :value="product.quantity" @input="changeInputQuantity(product, $event.target)">
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  props: {
    products: {
      type: Array,
      default: () => {
        return [];
      }
    }
  },

  methods: {
    changeInputQuantity(product, target) {
      const oldQuantity = product.quantity;
      const newQuantity = new Number(target.value);
      if (newQuantity <= 0) {
        target.value = oldQuantity;
        return;
      }

      const isIncrease = this.isIncrease(newQuantity, oldQuantity)
      const differenceValue = newQuantity - oldQuantity;
      if (isIncrease && differenceValue > product.stock) {
        target.value = oldQuantity;
        alert('Out of stock');
        return;
      }

      this.updateStock(product, newQuantity, oldQuantity);
      this.$set(product, 'quantity', newQuantity);
    },
  
    updateStock(product, newValue, oldValue) {
      if (newValue == oldValue) {
        return;
      }

      if (!this.isIncrease(newValue, oldValue)) {
        const differenceValue = oldValue - newValue;
        this.$emit('increaseStock', product, differenceValue);
        return;
      }

      const differenceValue = newValue - oldValue;
      this.$emit('decreaseStock', product, differenceValue);
      return;
    },
  
    isIncrease(newValue, oldValue) {
      return newValue > oldValue;
    }
  },

  computed: {
    hasProductsOnCart() {
      return this.products.length != 0;
    },
  }
}
</script>
<style scoped>
  table{
    min-width: 80%;
    margin:auto;
  }

  table, th, tr, td {
    border: 2px solid black;
    border-collapse: collapse;
  }

  .main {
    border: 2px solid black;
    min-width: 80%;
    margin: auto;
  }
  
  .cartTitle {
   text-align: center;
  }
  .cartMessage {
    text-align: center;
  }
  .cartTitle p {
     border-bottom: 0.5px solid gray;
  }

  img {
    max-width: 100px;
  }
</style>