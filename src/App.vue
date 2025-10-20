<script setup>
import Table from "./components/Table.vue";
import { provide, reactive } from "vue";
import { initialOrderPositions } from "./data";
const TABLES_NUMBER = 10;

const tables = reactive(
  Array(TABLES_NUMBER)
    .fill(0)
    .map(() => JSON.parse(JSON.stringify(initialOrderPositions)))
);
provide("tables", tables);
const bills = reactive(
  Array(TABLES_NUMBER)
    .fill(0)
    .map(() => JSON.parse(JSON.stringify([])))
);
provide("bills", bills);
</script>

<template>
  <main>
    <article>
      <h1>Tables</h1>
      <div class="container">
        <Table v-for="(table, i) in tables" :table="table" :table-id="i" />
      </div>
    </article>
  </main>
</template>

<style>
h1 {
  text-align: center;
  font-size: 3rem;
  font-weight: 600;
  margin-top: 0.6em;
}

.container {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 4vw;
  max-width: 100%;
  padding: 3em;
}
</style>
