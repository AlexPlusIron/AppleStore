<script setup>
import './LowBanner.css';
</script>
<template>
  <div class="low-banner">
    <div class="low-banner-content container-flex-row">
      <div class="left-container container-flex-column">
        <div class="low-banner-placeholder01 container-flex-row"> 
          <div class="image-placeholder">
            <img src="../assets/img/PlayStation5.jpg" alt="Playstation 5" />
          </div>
          <div class="text-placeholder">
            <h2>Playstation 5</h2>
            <p>Incredibly powerful CPUs, GPUs, and an SSD with integrated I/O will redefine your PlayStation experience.
            </p>
          </div>
        </div>
        <div class="low-banner-placeholder02 container-flex-row">
          <div class="placeholder-item"></div>
          <div class="placeholder-item"></div>
        </div>
      </div>
      <div class="right-container container-flex-row">
        <div class="low-banner-text-content">
          <h1 class="low-banner-iphone-title">Macbook Air</h1>
          <p class="low-banner-description">
            <em>The new 15-inch MacBook Air makes room for more of what you
              love with a spacious Liquid Retina display.</em>
          </p>
          <button class="low-banner-shop-button">Shop now</button>
        </div>
        <div class="low-banner-image-content">
          <img src="../assets/img/MacBook02.jpg" alt="iPhone 14 Pro" />
        </div>
      </div>
    </div>
  </div>
</template>