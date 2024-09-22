<template>
  <div :class="pageClass" id="app">
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
      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.index}`
        );
        const product = await response.json();

        // Ensure that valid products are set
        if (
          product.category === "men's clothing" ||
          product.category === "women's clothing"
        ) {
          this.product = product;
        } else {
          this.product = null;
        }

        // Update index for the next product
        this.index = this.index >= 20 ? 1 : this.index + 1;
      } catch (error) {
        console.error("Error fetching product:", error);
      }
    },
  },
  created() {
    this.fetchNextProduct(); // Fetch the first product when the component is created
  },
};
</script>

<!-- External CSS file -->
<style src="./assets/style/styles.css"></style>
