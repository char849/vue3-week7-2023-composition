<template>
  <h2>訂單列表</h2>
  <table class="table mt-6">
    <thead>
      <tr>
        <th>購買時間</th>
        <th>Email</th>
        <th>購買款項</th>
        <th>付款方式</th>
        <th>應付金額</th>
        <th>是否付款</th>
        <th>編輯</th>
      </tr>
    </thead>
    <tbody>
      <template v-for="(item, key) in orders" :key="key">
        <tr v-if="orders.length" :class="{ 'text-danger': !item.is_paid }">
          <td>{{ $filters.date(item.create_at) }}</td>
          <td><span v-text="item.user.email" v-if="item.user"></span></td>
          <td>
            <ul class="list-unstyled">
              <li v-for="(product, index) in item.products" :key="index">
                {{ product.product.title }} 數量：{{ product.qty }}
                {{ product.product.unit }}
              </li>
            </ul>
          </td>
          <td><span v-text="item.user.payMethod" v-if="item.user"></span></td>
          <td class="text-right">{{ $filters.currency(item.total) }}</td>
          <td>
            <div class="form-check form-switch">
              <input
                class="form-check-input"
                type="checkbox"
                :id="`${item.id}`"
                v-model="item.is_paid"
              />
              <label class="form-check-label" :for="`${item.id}`">
                <span v-if="item.is_paid">已付款</span>
                <span v-else>未付款</span>
              </label>
            </div>
          </td>
          <td>
            <div class="btn-group">
              <button
                class="btn btn-outline-dark btn-sm"
                type="button"
                @click="openOrdersModal(item)"
              >
                檢視
              </button>
              <button
                class="btn btn-outline-secondary btn-sm"
                type="button"
                @click="openDelOrderModal(item)"
              >
                刪除
              </button>
            </div>
          </td>
        </tr>
      </template>
    </tbody>
  </table>
  <!-- 分頁元件 -->
  <Pagination :pages="pagination" @get-product="getOrders" />
</template>

<script setup>
import axios from 'axios';

import { ref } from 'vue';
import Pagination from '@/components/PaginationComponent.vue';

const { VITE_API, VITE_PATH } = import.meta.env;

const orders = ref([]);
const pagination = ref({});

const getOrders = (page = 1) => {
  const url = `${VITE_API}api/${VITE_PATH}/admin/orders?page=${page}`;
  axios
    .get(url)
    .then((res) => {
      orders.value = res.data.orders;
      pagination.value = res.data.pagination;
    })
    .catch((err) => {
      this.$swal(err.response, '', 'error');
    });
};

getOrders();
</script>
