<template>
    <div class="container shopping-cart">
      <div class="container-flex-row">
        <div class="cart-items container-flex-column">
          <h1>Shopping Cart</h1>
          <div v-for="(product, index) in getProductsInCart.values()" :key="index" class="cart-item container-flex-row">
            <router-link :to="`/api/product/${product.product.id}`">
              <img style="height: 128px" :src="buildPath(product.product.images[0])">
            </router-link>
            <div class="item-title">{{ product.product.name }}</div>
            <div class="item-counter container-flex-row">
              <button class="count-down" @click="decreaseQuantity(product.product)">
                <img :src="minus">
              </button>
              <input class="count-input" type="tel" :value="product.count" @input="updateQuantity(product.product, $event.target.value)">
              <button class="count-up" @click="increaseQuantity(product.product)">
                <img :src="plus">
              </button>
            </div>
            <div class="item-price container-flex-row">
              <p>${{ itemTotalPrice(product) }}</p>
            </div>
          </div>
        </div>
        <div class="cart-summary container-flex-column">
          <h2>Order Summary</h2>
          <div class="cart-total container-flex-row">
            <p style="padding-right: 20%; padding-top: 1rem;">Total</p>
            <p>${{ totalPrice }}</p>
          </div>
          <button class="cart-checkout">Checkout</button>
        </div>
      </div>
    </div>
  </template>
  
  
  
  <script setup>
  import { ref, computed } from 'vue';
  import { useStore } from 'vuex';
  import { buildPath } from '../api.js';
  import plus from '../assets/img/cart/plus.svg';
  import minus from '../assets/img/cart/minus.svg';
  import './CartView.css';
  
  const store = useStore();
  
  const getProductsInCart = computed(() => store.getters.getProductsInCart);
  
  const totalPrice = computed(() => {
    return Array.from(getProductsInCart.value.values()).reduce((total, item) => {
      return total + (item.product.discount_price || item.product.price) * item.count;
    }, 0);
  });
  
  const addProduct = (product) => store.dispatch('addProduct', product);
  const removeProduct = (product) => store.dispatch('removeProduct', product);
  
  const increaseQuantity = (product) => {
    addProduct(product);
  };
  
  const decreaseQuantity = (product) => {
    removeProduct(product);
  };
  
  const updateQuantity = (product, quantity) => {
    quantity = parseInt(quantity);
    const currentQuantity = getProductsInCart.value.get(product.id).count;
  
    if (quantity > currentQuantity) {
      for (let i = 0; i < quantity - currentQuantity; i++) {
        addProduct(product);
      }
    } else {
      for (let i = 0; i < currentQuantity - quantity; i++) {
        removeProduct(product);
      }
    }
  };
  
  const itemTotalPrice = (item) => {
    return (item.product.discount_price || item.product.price) * item.count;
  };
  </script>
  
  