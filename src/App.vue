<template>
  <div id="app">
    <h1>Product Viewer</h1>
    <ProductCard 
      v-if="product"
      :product="product"
    />
    <button @click="nextProduct">Next Product</button>
  </div>
</template>

<script>
import ProductCard from './components/ProductCard.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    ProductCard
  },
  data() {
    return {
      product: null,
      index: 1
    };
  },
  methods: {
    fetchProduct() {
      axios.get(`https://fakestoreapi.com/products/${this.index}`)
        .then(response => {
          const product = response.data;
          if (product.category === "men's clothing" || product.category === "women's clothing") {
            this.product = product;  // Simpan produk jika kategori sesuai
          } else {
            this.product = null;  // Tidak simpan produk lain
          }
        })
        .catch(error => {
          console.error(error);
        });
    },
    nextProduct() {
      this.index = this.index < 20 ? this.index + 1 : 1;  // Reset ke 1 jika index melebihi 20
      this.fetchProduct();
    }
  },
  mounted() {
    this.fetchProduct();  // Fetch produk pertama kali
  }
};
</script>

<style src="./styles.css"></style>
