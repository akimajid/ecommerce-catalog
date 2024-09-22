<template>
  <section :class="sectionClass">
    <div v-if="loading" class="loading-spinner">
      <span>Loading...</span>
    </div>
    <div v-else>
      <div v-if="product" class="product-card">
        <img :src="product.image" alt="Product Image" class="product-image" />
        <div class="product-info">
          <h2 class="product-title">{{ product.title }}</h2>
          <div class="category-rating">
            <span>{{ product.category }}</span>
            <span class="rating">
              <span class="rating-number">{{ product.rating.rate }}/5</span>
              <span
                v-for="n in 5"
                :key="n"
                :class="['circle', { filled: n <= product.rating.rate }]"
              ></span>
            </span>
          </div>
          <hr class="divider" />
          <p class="product-description">{{ product.description }}</p>
          <hr class="divider" />
          <p class="product-price">${{ product.price }}</p>
          <div class="button-group">
            <button class="btn buy-now" @click="buyNow">Buy now</button>
            <button class="btn next-product" @click="handleNextProduct">
              Next product
            </button>
          </div>
        </div>
      </div>
      <div v-else class="unavailable-product">
        <p>This product is unavailable to show</p>
        <button class="btn next-product" @click="handleNextProduct">
          Next product
        </button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    product: Object,
  },
  data() {
    return {
      loading: false,
    };
  },
  computed: {
    sectionClass() {
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
    buyNow() {
      alert("Product purchased!");
    },
    handleNextProduct() {
      this.loading = true;
      setTimeout(() => {
        this.$emit("fetchNextProduct");
        this.loading = false;
      }, 500);
    },
  },
};
</script>

<style scoped src="../assets/style/productCard.css"></style>
