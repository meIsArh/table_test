<template>
  <div class="pagination">
    <div class="pagination__item">
      <button
          @click="setPage(1)"
          :disabled="isCurrentFirstPage"
      >
        1
      </button>
    </div>

    <div class="pagination__item">
      <button
          @click="setPrevPage"
          :disabled="isCurrentFirstPage"
      >
        назад
      </button>
    </div>

    <div v-for="(page, index) in pages"
         :key="index + '_pagenumber'"
         class="pagination__item"
    >
      <button
          @click="setPage(page.number)"
          :disabled="page.disabled"
      >
        {{ page.number }}
      </button>
    </div>

    <div class="pagination__item">
      <button
          @click="setNexPage"
          :disabled="isCurrentLastPage"
      >
        вперед
      </button>
    </div>

    <div class="pagination__item">
      <button
          @click="setPage(totalCountPages)"
          :disabled="isCurrentLastPage"
      >
        {{ totalCountPages }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PaginationComponent",
  props: {
    totalItems: {
      type: Number,
      required: true,
    },
    visibleButtonsCount: {
      type: Number,
      required: false,
      default: 3,
    },
    maxItemsCountPerPage: {
      type: Number,
      required: false,
      default: 10,
    },
    currentPage: {
      type: Number,
      required: true,
    },
  },
  computed: {
    totalCountPages() {
      return this.totalItems / this.maxItemsCountPerPage;
    },
    isCurrentFirstPage() {
      return this.currentPage === 1;
    },
    isCurrentLastPage() {
      return this.currentPage === this.totalCountPages;
    },
    startPage() {
      if (this.currentPage === 1) {
        return 1;
      }
      if (this.currentPage === this.totalCountPages) {
        return this.totalCountPages - this.visibleButtonsCount + 1;
      }
      return this.currentPage - 1;
    },
    lastPage() {
      return this.startPage + this.visibleButtonsCount - 1;
    },
    pages() {
      const arr_pages = [];

      for (let i = this.startPage; i <= this.lastPage; i++) {
        arr_pages.push({
          number: i,
          disabled: i === this.currentPage
        });
      }
      return arr_pages;
    }
  },
  methods: {
    setPage(page) {
      this.$emit('pagechange', page);
    },
    setPrevPage() {
      this.$emit('pagechange', this.currentPage - 1);
    },
    setNexPage() {
      this.$emit('pagechange', this.currentPage + 1);
    },
  }
}
</script>

<style scoped>

</style>
