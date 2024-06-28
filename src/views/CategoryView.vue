<template>
    <div class="category-container flex-column">
      <div v-if="showNotification" class="notification">
        Product added to cart!
      </div>
      <div class="category-menu"></div>
      <div v-if="products" class="category-content">
        <div class="category-header">
          <h2 class="category-title">
            Selected Products: {{ products.length }}
          </h2>
          <select
            name="product-opts"
            id="product-select"
            class="category-select"
            v-model="sortBy"
          >
            <option value="default">--Please choose an option--</option>
            <option value="rating">By rating</option>
            <option value="price">By price</option>
          </select>
        </div>
        <div class="category-body">
          <ProductCard
            v-for="product in sortedProducts"
            :key="product.id"
            :product="product"
            @addToCart="addProductToCart"
          />
        </div>
        <div class="category-pagination"></div>
      </div>
      <div v-else-if="isLoading">Loading products...</div>
      <div v-else-if="fetchError">Error: {{ fetchError }}</div>
    </div>
  </template>
  
  <script>
  import { mapActions } from 'vuex';
  import { ref, watch, onMounted, computed } from 'vue';
  import { useRoute } from 'vue-router';
  import { getCategory } from '../api.js';
  import ProductCard from './ProductCard.vue';
  import './CategoryView.css';
  
  export default {
    components: {
      ProductCard,
    },
    setup() {
      const route = useRoute();
      const isLoading = ref(false);
      const products = ref(null);
      const fetchError = ref(null);
      const showNotification = ref(false);
      const sortBy = ref('default');
  
      const fetchProducts = async (id) => {
        isLoading.value = true;
        products.value = null;
        fetchError.value = null;
  
        try {
          products.value = await getCategory(id);
        } catch (err) {
          fetchError.value = err.toString();
        } finally {
          isLoading.value = false;
        }
      };
  
      onMounted(() => {
        fetchProducts(route.params.id);
      });
  
      watch(
        () => route.params.id,
        (newId) => {
          fetchProducts(newId);
        }
      );
  
      const sortedProducts = computed(() => {
        if (sortBy.value === 'price') {
          return [...products.value].sort((a, b) => a.price - b.price);
        } else if (sortBy.value === 'rating') {
          return [...products.value].sort((a, b) => b.rating - a.rating);
        } else {
          return products.value; 
        }
      });
  
      return {
        isLoading,
        products,
        fetchError,
        showNotification,
        fetchProducts,
        sortBy,
        sortedProducts,
      };
    },
    methods: {
      ...mapActions(['addProduct']),
      addProductToCart(product) {
        this.addProduct(product)
          .then(() => {
            this.showNotification = true;
            setTimeout(() => {
              this.showNotification = false;
            }, 3000);
          })
          .catch((error) => {
            console.error('Ошибка при добавлении товара в корзину:', error);
          });
      },
    },
  };
  </script>
  