<template>
  <div class="container">
    <h2>後台登入頁面</h2>
    <div class="row">
      <div class="col-12">
        <form class="form-signin" @submit.prevent="login">
          <div class="form-floating">
            <input
              type="email"
              class="form-control"
              v-model="user.username"
              id="floatingInput"
              placeholder="Password"
              required
            />
            <label for="floatingInput">Email address</label>
          </div>
          <div class="form-floating">
            <input
              type="password"
              class="form-control"
              v-model="user.password"
              id="floatingPassword"
              placeholder="Password"
              required
            />
            <label for="floatingPassword">Password</label>
          </div>
          <button
            class="btn btn-lg btn-primary w-100 mt-3"
            type="button"
            @click="login"
          >
            登入
          </button>
        </form>
      </div>
    </div>
    <p class="mt-5 mb-3 text-muted">
      &copy; 2024 - Charlotte Lee<br />
      vue 第六週主線任務 建立路由表
      <router-link class="nav-link" to="/">返回首頁</router-link>
    </p>
  </div>
</template>

<script setup>
import axios from 'axios';

import { ref } from 'vue';
import { useRouter } from 'vue-router';

const { VITE_API } = import.meta.env;

const router = useRouter();

const user = ref({});

const login = () => {
  const api = `${VITE_API}admin/signin`;
  axios
    .post(api, user.value)
    .then((res) => {
      const { token, expired } = res.data;
      console.log(token, expired);
      document.cookie = `hexToken=${token};expires=${new Date(expired)}`;
      router.push('/admin/products');
    })
    .catch((err) => {
      alert(err.response.data.message);
    });
};
</script>
