<template>
    <div class="product-card">
      <router-link :to="`/api/product/${product.id}`">
        <div class="product-like">
          <img src="../assets/img/products/fav.svg" alt="like" />
        </div>
        <div class="product-image">
          <img :src="buildPath(product.images[0])" alt="product image" style="width: 70%;" />
        </div>
        <p class="product-name">{{ product.name }}</p>
        <div class="product-price">${{ product.price }}</div>
      </router-link>
      <button class="product-action" @click="addToCart(product)">
        Buy Now
      </button>
    </div>
  </template>
  
  <script>
  import { buildPath } from '../api.js';
  //import './ProductView.css';
  
  export default {
    props: {
      product: {
        type: Object,
        required: true,
      },
    },
    emits: ['addToCart'],
    methods: {
      buildPath,
      addToCart(product) {
        this.$emit('addToCart', product);
      },
    },
  };
  </script>