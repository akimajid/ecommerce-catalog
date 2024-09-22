<template>
  <div id="app">
    <Product
      :product="product"
      :index="index"
      @fetchNextProduct="fetchNextProduct"
    />
  </div>
</template>

<script>
import Product from "./components/ProductCard.vue";

export default {
  data() {
    return {
      index: 1,
      product: null,
    };
  },
  components: {
    Product,
  },
  methods: {
    async fetchNextProduct() {
      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.index}`
        );
        const product = await response.json();

        if (
          product.category === "men's clothing" ||
          product.category === "women's clothing"
        ) {
          this.product = product;
        } else {
          this.product = null;
        }
        this.index = this.index >= 20 ? 1 : this.index + 1;
      } catch (error) {
        console.error("Error fetching product:", error);
      }
    },
  },
  created() {
    this.fetchNextProduct();
  },
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
}

:root {
  --men-bg: #d6e6ff;
  --women-bg: #fde2ff;
  --unavailable-bg: #dcdcdc;
  --primary-color: #720060;
  --secondary-color: #002772;
  --price-color: #02939e;
}
</style>
