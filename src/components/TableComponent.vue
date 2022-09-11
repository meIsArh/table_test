<template>
  <div class="table">
    <div class="table__head">
      <input type="text" placeholder="поиск" v-model="search" @input="searchTable">
    </div>

    <table>
      <thead>
      <tr>
        <th v-for="(item, index) in tableHeadKeys" :key="index + '_th'">
          <div class="table__arrowcont">
            <span>{{ item }}</span>

            <span class="table__arrow"
                  :class="sortableKey === item && sortableName === 'asc' ? 'table__arrow--active' : ''"
                  @click="sortTable(item, 'asc')">↑</span>

            <span class="table__arrow"
                  :class="sortableKey === item && sortableName === 'desc' ? 'table__arrow--active' : ''"
                  @click="sortTable(item, 'desc')">↓</span>
          </div>
        </th>
      </tr>
      </thead>

      <tbody>
      <tr v-for="(item, index) in paginateDataTable" :key="index + '_tr'">
        <td v-for="(el, i) in item" :key="i + '_td'">{{ el }}</td>
      </tr>
      </tbody>
    </table>

    <pagination
        v-if="!search"
        :total-items="totalItems"
        :max-items-count-per-page="10"
        :current-page="currentPage"
        @pagechange="pageChange"
    />
  </div>
</template>

<script>
import Pagination from "@/components/Pagination";


export default {
  name: "TableComponent",
  components: {Pagination},
  props: {
    data: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  computed: {
    tableHeadKeys() {
      return Object.keys(this.data[0]);
    },
    totalItems() {
      return Number(this.dataTable.length);
    },
    paginateDataTable() {
      if (this.search) return this.dataTable;

      if (this.currentPage === 1) {
        return this.dataTable.slice(0, 10)
      }
      return this.dataTable.slice(this.currentPage * 10 - 10, this.currentPage * 10);
    }
  },
  created() {
    this.setData();
  },
  data: () => ({
    search: '',
    sortableKey: '',
    sortableName: '',
    dataTable: null,
    currentPage: 1,
  }),
  methods: {
    pageChange(page) {
      console.log(page)
      this.currentPage = page;
    },
    sortTable(key, sortName) {
      if (sortName === this.sortableName) return this.clearSearchAndSort();

      this.sortableKey = key;
      this.sortableName = sortName;

      this.dataTable.sort((a, b) => {
        let nameA = a[key];
        let nameB = b[key];

        if (key !== 'id') {
          nameA = a[key].toString().toLowerCase();
          nameB = b[key].toString().toLowerCase();
        }

        if (sortName === 'asc') {
          if (nameA < nameB)
            return -1
          if (nameA > nameB)
            return 1
          return 0;
        }

        if (sortName === 'desc') {
          if (nameA > nameB)
            return -1
          if (nameA < nameB)
            return 1
          return 0;
        }

      })
    },
    searchTable() {
      const names = Object.keys(this.data[0]);

      const array = names.map(name => this.data.filter(
          item => item[name].toString().toLowerCase().includes(this.search)
      )).flat();

      this.dataTable = Array.from(new Set(array));
    },
    clearSearchAndSort() {
      this.setData();
      this.sortableKey = '';
      this.sortableName = '';
      this.search = '';
    },
    setData() {
      this.dataTable = Object.assign([], this.data);
      console.log(this.dataTable);
    }
  }
}
</script>
