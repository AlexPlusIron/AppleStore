
<script setup>
import './Categories.css';
</script>

<template>
<div class="categories container container-flex-column">
    <div class="categories-nav container-flex-row">
        <h2>Browse By Category</h2>
        <div class="nav-arrows">
            <img src="../assets/img/category/arrow_left.svg" style="padding-left: 32px;">
            <img src="../assets/img/category/arrow_right.svg" style="padding-left: 32px;">
        </div>
    </div>
    <div class="categories-cnt container-flex-row">
        <div class="category-card"><router-link to="/api/category/2"><img src="../assets/img/category/phones.svg"><p>Phones</p></router-link></div>
        <div class="category-card"><router-link to="/api/category/5"><img src="../assets/img/category/smartwatches.svg"><p>Smart Watches</p></router-link></div>
        <div class="category-card"><router-link to="/api/category/2"><img src="../assets/img/category/cameras.svg"><p>Cameras</p></router-link></div>
        <div class="category-card"><router-link to="/api/category/6"><img src="../assets/img/category/headphones.svg"><p>Headphones</p></router-link></div>
        <div class="category-card"><router-link to="/api/category/3"><img src="../assets/img/category/computers.svg"><p>Computers</p></router-link></div>
        <div class="category-card"><router-link to="/api/category/4"><img src="../assets/img/category/gaming.svg"><p>Gaming</p></router-link></div>
        <router-view></router-view>
    </div>
</div>
</template>

