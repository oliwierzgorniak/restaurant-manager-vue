<script setup>
import OptionMenu from "./Table/OptionMenu.vue";
import { provide, ref } from "vue";

defineProps(["tableId", "table"]);
const isOptionMenuOpen = ref(false);
provide("isOptionMenuOpen", isOptionMenuOpen);
const tableState = ref("Free");
provide("tableState", tableState);
</script>

<template>
  <div class="table" @click="isOptionMenuOpen = true">
    <span
      class="table__state"
      :style="{ opacity: tableState === 'Free' ? 0.5 : 1 }"
      >{{ tableState }}</span
    >
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
  border: 0.3em solid var(--text-color);
  width: 100%;
  aspect-ratio: 3/2;
  font-size: 1.1rem;
  font-weight: 550;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.table__state {
  font-size: 1.5rem;
  text-decoration: underline;
}
</style>
