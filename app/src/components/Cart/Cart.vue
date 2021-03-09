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
        <tr v-for="product in products" :key="product.id" @click="addProductOnCart(product)">
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
            <input type="number">
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

  computed: {
    hasProductsOnCart() {
      return this.products.length != 0;
    }
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