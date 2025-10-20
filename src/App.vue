<script setup lang="ts">
import Table from "./components/Table.vue";
import { provide, reactive } from "vue";
import { initialOrderPositions } from "./data";
const TABLES_NUMBER = 10;
const tablesSeed = Array(TABLES_NUMBER)
  .fill(null)
  .map(() => ({ ...initialOrderPositions }));
const tables = reactive(tablesSeed);
provide("tables", tables);
</script>

<template>
  <div class="container">
    <Table v-for="(table, i) in tables" :table="table" :table-id="i" />
  </div>
</template>

<style>
.container {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 4vw;
  max-width: 100%;
}
</style>
