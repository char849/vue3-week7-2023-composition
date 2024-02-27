<!-- eslint-disable import/extensions -->
<template>
  <div class="container">
    <h2>產品列表</h2>
    <div class="container">
      <div class="row row-cols-1 row-cols-lg-4 g-3">
        <div class="col" v-for="product in products" :key="product.id">
          <div class="card h-100">
            <img :src="product.imageUrl" class="card-img-top" alt="..." />
            <div class="card-body">
              <h5 class="card-title">{{ product.title }}</h5>
              <p class="card-text">
                {{ product.description }}
              </p>
              <div class="h5" v-if="product.price === product.origin_price">
                {{ product.price }} 元
              </div>
              <div v-else>
                <del class="h6">原價 {{ product.origin_price }} 元</del>
                <p class="h5">現在只要 {{ product.price }} 元</p>
              </div>
            </div>
            <div class="card-footer bg-white">
              <div class="d-grid gap-2 text-center d-md-block">
                <router-link
                  class="btn my-2 me-2 btn-outline-primary btn-sm"
                  type="button"
                  :to="`/product/${product.id}`"
                >
                  查看更多
                </router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';

const { VITE_API, VITE_PATH } = import.meta.env;

const products = ref([]);

const getProducts = () => {
  axios
    .get(`${VITE_API}api/${VITE_PATH}/products/all`)
    .then((res) => {
      products.value = res.data.products;
    })
    .catch((err) => {
      alert(err.response.data.message);
    });
};

onMounted(() => {
  getProducts();
});
</script>
