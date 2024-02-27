<template>
  <div class="container">
    <div class="row">
      <h2>單一產品頁面</h2>
      <div class="col-sm-6">
        <img class="img-fluid" :src="product.imageUrl" alt="" />
      </div>
      <div class="col-sm-6">
        <span class="badge bg-primary rounded-pill">{{
          product.category
        }}</span>
        <p>商品描述：{{ product.description }}</p>
        <p>商品內容：{{ product.content }}</p>
        <div class="h5">{{ product.origin_price }} 元</div>
        <del class="h6">原價 {{ product.origin_price }} 元</del>
        <div class="h5">現在只要 {{ product.price }} 元</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const { VITE_API, VITE_PATH } = import.meta.env;
const route = useRoute();

const product = ref({});

const getProduct = () => {
//   console.log('route:', route);
  const { id } = route.params;
  axios.get(`${VITE_API}api/${VITE_PATH}/product/${id}`).then((res) => {
    // console.log(res.data.product);
    // 將遠端資料取回
    product.value = res.data.product;
  });
};

onMounted(() => {
  getProduct();
});
</script>
