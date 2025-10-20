<script setup>
import { allergens } from "@/data";
const emits = defineEmits(["update:areAllergensOpen"]);
const props = defineProps({
  positionName: String,
});
</script>

<template>
  <div className="{styles.allergensModal}">
    <ul>
      <li v-if="allergens[positionName].length > 0" v-for="allergen in allergens[positionName]">
        {{ allergen }}
      </li>
      <li v-else>No allergens</li>
    </ul>
    <button
      @click="
        (e) => {
          e.preventDefault();
          emits('update:areAllergensOpen', false);
        }
      "
      className="{styles.closeButton}"
    >
      Close
    </button>
  </div>
</template>
