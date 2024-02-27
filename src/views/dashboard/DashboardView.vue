<template>
  <h2>這是後台</h2>
  <nav>
    <RouterLink to="/admin/products">產品列表</RouterLink> |
    <RouterLink to="/admin/order">訂單列表</RouterLink> |
    <RouterLink to="/">回到前台</RouterLink>
  </nav>
  <router-view></router-view>
</template>

<script setup>
import { useRouter } from 'vue-router';
import axios from 'axios';
import { onMounted } from 'vue';

const router = useRouter();

const { VITE_API } = import.meta.env;

const checkAdmin = () => {
  const url = `${VITE_API}api/user/check`;
  axios
    .post(url)
    .then((res) => {
      console.log('驗證成功：', res.data.success);
    })
    .catch(() => {
      router.push('/login');
    });
};

onMounted(() => {
  const token = document.cookie.replace(
    /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/,
    '$1',
  );
  axios.defaults.headers.common.Authorization = token;
  checkAdmin();
});

</script>
