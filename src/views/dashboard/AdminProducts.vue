<template>
  <h2>產品列表</h2>
  <div class="container">
    <div class="text-end mt-4">
      <button class="btn btn-primary" @click="openModal('new')">
        建立新課程
      </button>
    </div>
    <table class="table mt-4">
      <thead>
        <tr>
          <th width="120">課程分類</th>
          <th>課程名稱</th>
          <th width="120">課程原價</th>
          <th width="120">課程售價</th>
          <th width="100">是否上架</th>
          <th width="120">編輯課程</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in products" :key="item.id">
          <td>{{ item.category }}</td>
          <td>{{ item.title }}</td>
          <td class="text-end">{{ item.origin_price }}</td>
          <td class="text-end">{{ item.price }}</td>
          <td>
            <span v-if="item.is_enabled" class="text-success">啟用</span>
            <span v-else>未啟用</span>
          </td>
          <td>
            <div class="btn-group">
              <button
                type="button"
                class="btn btn-outline-primary btn-sm"
                @click="openModal('edit', item)"
              >
                編輯
              </button>
              <button
                type="button"
                class="btn btn-outline-danger btn-sm"
                @click="openModal('delete', item)"
              >
                刪除
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- 分頁元件 -->
    <PaginationComponent
      :pages="pagination"
      @emit-pages="getData"
    ></PaginationComponent>
    <!-- 分頁元件 -->
  </div>
  <!-- Modal -->
  <ProductModal
    :temp-product="tempProduct"
    :is-new="isNew"
    @get-data="getData"
    ref="productModalRef"
  ></ProductModal>
  <DelModal
    :temp-product="tempProduct"
    @get-data="getData"
    ref="delModalRef"
  ></DelModal>
</template>

<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';

import DelModal from '@/components/DelModal.vue';
import PaginationComponent from '@/components/PaginationComponent.vue';
import ProductModal from '@/components/ProductModal.vue';

const { VITE_API, VITE_PATH } = import.meta.env;

const products = ref([]);
const isNew = ref(false);
const tempProduct = ref({
  imagesUrl: [],
});
const pagination = ref({});
const productModalRef = ref(null);
const delModalRef = ref(null);

// 參數預設值 pgae , query -> ?page=${}
const getData = (page = 1) => {
  const url = `${VITE_API}api/${VITE_PATH}/admin/products/?page=${page}`;
  axios
    .get(url)
    .then((res) => {
      products.value = res.data.products;
      pagination.value = res.data.pagination;
    })
    .catch((err) => {
      alert(err.response.data.message);
    });
};

const openModal = (type, item) => {
  if (type === 'new') {
    tempProduct.value = {
      imagesUrl: [],
    };
    isNew.value = true;
    productModalRef.value.openModal();
  } else if (type === 'edit') {
    tempProduct.value = { ...item };
    isNew.value = false;
    productModalRef.value.openModal();
  } else if (type === 'delete') {
    tempProduct.value = { ...item };
    delModalRef.value.openModal();
  }
};

onMounted(() => {
  const token = document.cookie.replace(
    /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/,
    '$1',
  );
  axios.defaults.headers.common.Authorization = token;
  getData();
});
</script>
