<template>
  <div id="app">
    <table-component
        v-if="getDataTable"
        :data="getDataTable"
    />
  </div>
</template>

<script>
import table_data from './API/data.json';
import TableComponent from "@/components/TableComponent";

export default {
  name: 'App',
  components: {TableComponent},

  computed: {
    getDataTable() {
      return table_data;
    }
  }
}
</script>

<style>

</style>
