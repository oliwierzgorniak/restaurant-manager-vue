<script setup lang="ts">
import OptionMenu from "./OptionMenu.vue";
import { initialOrderPositions } from "@/data";
import { provide, ref } from "vue";

defineProps<{
  table: typeof initialOrderPositions;
  tableId: number;
}>();
const isOptionMenuOpen = ref(false);
provide("isOptionMenuOpen", isOptionMenuOpen);
const tableState = ref("Free");
provide("tablesState", tableState);
const bill = ref([]);
provide("bill", bill);
</script>

<template>
  <div class="table" @click="isOptionMenuOpen = true">
    <span>{{ tableState == "Free" ? "" : tableState }}</span>
  </div>
  <OptionMenu
    v-if="isOptionMenuOpen"
    v-model:tableState="tableState"
    v-model:isOptionMenuOpen="isOptionMenuOpen"
    :tableId
  />
</template>

<style @scoped>
.table {
  aspect-ratio: 3/2;
  background-color: green;
}
</style>
