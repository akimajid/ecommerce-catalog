<template>
  <div :class="pageClass" id="app">
    <Product
      :product="product"
      @fetchNextProduct="fetchNextProduct"
      :loading="loading"
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
      loading: false,
    };
  },
  components: {
    Product,
  },
  computed: {
    pageClass() {
      if (this.product && this.product.category === "men's clothing") {
        return "men-section";
      } else if (this.product && this.product.category === "women's clothing") {
        return "women-section";
      } else {
        return "unavailable-section";
      }
    },
  },
  methods: {
    async fetchNextProduct() {
      this.loading = true;
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
      } finally {
        this.loading = false;
      }
    },
  },
  created() {
    this.fetchNextProduct();
  },
};
</script>

<!-- External CSS file -->
<style src="./assets/style/styles.css"></style>
