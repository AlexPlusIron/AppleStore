<script setup>
import { ref, watch } from 'vue';
import { useRoute } from 'vue-router';
import { useStore } from 'vuex';
import { getProduct, buildPath } from '../api.js';
import screensize from '../assets/img/product/info/specs/screensize.svg';
import cpu from '../assets/img/product/info/specs/cpu.svg';
import cores from '../assets/img/product/info/specs/cores.svg';
import battery from '../assets/img/product/info/specs/battery.svg';
import delivery from '../assets/img/product/info/misc/delivery.svg';
import stock from '../assets/img/product/info/misc/stock.svg';
import guarantee from '../assets/img/product/info/misc/guarantee.svg';
import './ProductView.css';

const route = useRoute();
const store = useStore();

const loading = ref(false);
const post = ref(null);
const error = ref(null);
const showNotification = ref(false);

const mapIconsSpec = new Map([
    ["Диагональ", screensize],
    ["Разрешение", screensize],
    ["Процессор", cpu],
    ["Аккумулятор", battery],
    ["Питание", battery],
    ["Кол-во ядер", cores]
]);

const getIcon = (elem) => {
    let res = mapIconsSpec.get(elem.characteristic);
    return res == undefined ? '' : res;
};

watch(() => route.params.id, fetchData, { immediate: true });

async function fetchData(id) {
    error.value = post.value = null;
    loading.value = true;

    try {
        post.value = await getProduct(id);
    } catch (err) {
        error.value = err.toString();
    } finally {
        loading.value = false;
    }
}

const addProductToCart = (product) => {
    store.dispatch('addProduct', product);
    showNotification.value = true;
    setTimeout(() => {
        showNotification.value = false;
    }, 3000);
};
</script>

<template>
    <div class="product-container">
        <div v-if="showNotification" class="notification">
            Product added to cart!
        </div>
        <div v-if="post" class="container container-flex-column">
            <div class="product-intro container-flex-row">
                <div class="product-images">
                    <img :src="buildPath(post.images[0])" />
                </div>
                <div class="product-info container-flex-column">
                    <div class="product-title">{{ post.name }}</div>
                    <div class="product-price container-flex-row">
                        <p>${{ post.discount_price ? post.discount_price : post.price }}</p>
                        <s v-if="post.discount_price">${{ post.price }}</s>
                    </div>
                    <div class="product-specs container-flex-row">
                        <div v-for="characteristic in post.characteristics" :key="characteristic.characteristic" class="product-spec container-flex-row">
                            <div class="ps-image">
                                <img :src="getIcon(characteristic)" />
                            </div>
                            <div class="ps-text">
                                <h3>{{ characteristic.characteristic }}</h3>
                                <p>{{ characteristic.value }}</p>
                            </div>
                        </div>
                    </div>
                    <div class="product-descr">{{ post.description }}</div>
                    <div class="product-actions container-flex-row">
                        <button class="action-wishlist">Add to Wishlist</button>
                        <button class="action-cart" @click="addProductToCart(post)">Add to Cart</button>
                    </div>
                    <div class="product-misc container-flex-row">
                        <div class="misc-delivery container-flex-row">
                            <div class="ms-image"><img :src="delivery" /></div>
                            <div class="ms-text container-flex-column">
                                <h3>Free Delivery</h3>
                                <p>1-2 days</p>
                            </div>
                        </div>
                        <div class="misc-in-stock container-flex-row">
                            <div class="ms-image"><img :src="stock" /></div>
                            <div class="ms-text container-flex-column">
                                <h3>In Stock</h3>
                                <p>{{ post.is_available ? 'Today' : 'None' }}</p>
                            </div>
                        </div>
                        <div class="misc-guaranteed container-flex-row">
                            <div class="ms-image"><img :src="guarantee" /></div>
                            <div class="ms-text container-flex-column">
                                <h3>Guaranteed</h3>
                                <p>1 year</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="product-details container-flex-column">
                <h2>Details</h2>
                <ul v-for="characteristic in post.characteristics" :key="characteristic.characteristic">
                    <li>
                        <div class="details-flex-row container-flex-row">
                            <p>{{ characteristic.characteristic }}</p>
                            <p>{{ characteristic.value }}</p>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="product-mark container-flex-column">
                <div class="product-mark-box">
                    <h3>{{ post.rating }}</h3>
                    <p>{{ `of ${post.count_review} reviews` }}</p>
                    <span :class="post.rating >= 1 ? 'fa fa-star checked' : 'fa fa-star'"></span>
                    <span :class="post.rating >= 2 ? 'fa fa-star checked' : 'fa fa-star'"></span>
                    <span :class="post.rating >= 3 ? 'fa fa-star checked' : 'fa fa-star'"></span>
                    <span :class="post.rating >= 4 ? 'fa fa-star checked' : 'fa fa-star'"></span>
                    <span :class="post.rating >= 5 ? 'fa fa-star checked' : 'fa fa-star'"></span>
                </div>
            </div>
        </div>
    </div>
</template>
