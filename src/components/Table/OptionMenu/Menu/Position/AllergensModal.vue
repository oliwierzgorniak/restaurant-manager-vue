<script setup>
import { allergens } from "@/data";
const emits = defineEmits(["update:areAllergensOpen"]);
const props = defineProps({
  positionName: String,
});
</script>

<template>
  <div class="allergensModal">
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
      class="closeButton"
    >
      Close
    </button>
  </div>
</template>

<style scoped>
.allergensModal {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: var(--background-color);
  padding: 1.5em 3em 2.4em 3em;
  border: 0.1em solid var(--text-color);
}

.allergensModal ul {
  padding-left: 0em;
}

.closeButton {
  font-size: 1.6rem;
  padding: 0.3em 0.7em;
  margin: auto;
  display: block;
  margin-top: 1.2em;
}
</style>
