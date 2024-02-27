<template>
  <nav aria-label="Page navigation example" class="py-3 d-flex justify-content-center">
    <ul class="pagination">
      <li class="page-item" :class="{ disabled: !pages.has_pre }">
        <a
          class="page-link"
          href="#"
          aria-label="Previous"
          @click.prevent="updatePage(pages.current_page - 1)"
        >
          <span aria-hidden="true">&laquo;</span>
        </a>
      </li>
      <li
        class="page-item"
        v-for="page in pages.total_pages"
        :key="page + 'page'"
        :class="{ active: page === pages.current_page }"
      >
        <a class="page-link" href="#" @click.prevent="updatePage(page)">{{
          page
        }}</a>
      </li>
      <li class="page-item" :class="{ disabled: !pages.has_next }">
        <a
          class="page-link"
          href="#"
          aria-label="Next"
          @click.prevent="updatePage(pages.current_page + 1)"
        >
          <span aria-hidden="true">&raquo;</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script setup>
// eslint-disable-next-line no-unused-vars
const props = defineProps({
  pages: Object,
});

const emits = defineEmits(['emitPages']);

const updatePage = (page) => {
  emits('emitPages', page);
};
</script>
