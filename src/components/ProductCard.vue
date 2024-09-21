<template>
  <div :class="sectionClass">
    <h2>{{ product.title }}</h2>
    <img :src="product.image" alt="Product Image" />
    <p>{{ product.description }}</p>
    <p><strong>Category:</strong> {{ product.category }}</p>
    <p><strong>Price:</strong> ${{ product.price }}</p>
  </div>
</template>

<script>
export default {
  name: "ProductCard",
  props: {
    product: Object,
  },
  computed: {
    sectionClass() {
      if (this.product.category === "men's clothing") {
        return "men-section";
      } else if (this.product.category === "women's clothing") {
        return "women-section";
      } else {
        return "unavailable-section";
      }
    },
  },
};
</script>
