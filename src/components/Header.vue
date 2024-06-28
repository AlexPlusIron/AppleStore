<template>
    <div class="header-top container container-flex-row">
      <div class="header-logo">
        <router-link to="/">
          <img src="../assets/img/header/logo_vector.svg" />
        </router-link>
      </div>
      <div class="header-search container-flex-row">
        <div class="header-image">
          <img src="../assets/img/header/search_vector.svg" />
        </div>
        <input type="search" class="header-input" title="Search" value="Search" />
      </div>
      <div class="header-icons container-flex-row">
        <div class="header-icon">
          <img src="../assets/img/header/favorites_vector.svg" />
        </div>
        <div class="header-icon">
          <router-link to="/cart" class="cart-icon">
            <img src="../assets/img/header/cart_vector.svg" />
            <span v-if="cartItemsCount > 0" class="cart-counter">{{ cartItemsCount }}</span>
          </router-link>
        </div>
      </div>
    </div>
    <div class="header-line"></div>
  </template>
  
  <script setup>
import { computed } from 'vue';
import { useStore } from 'vuex';
import './Header.css';

const store = useStore();

const cartItemsCount = computed(() => {
  const productsInCart = store.getters.getProductsInCart;
  return Array.from(productsInCart.values()).reduce((total, item) => total + item.count, 0);
});
</script>
  
  