<script setup lang="ts">
import RequestModal from "./Position/RequestModal.vue";
import AllergensModal from "./Position/AllergensModal.vue";
import type { initialOrderPositions } from "@/data";
import { ref, inject } from "vue";

const props = defineProps<{
  position: { name: string; price: number; ingredients: string[] };
  tableId: number;
}>();

const isRequestModalOpen = ref(false);
const areAllergensOpen = ref(false);
const tables = inject("tables") as (typeof initialOrderPositions)[];

const changeAmount = (n: number) => {
  const table = tables[props.tableId] as typeof initialOrderPositions;
  // @ts-ignore
  const currentAmount = table[props.position.name].amount;
  if ((currentAmount > 0 && n < 0) || n >= 0) {
    // @ts-ignore
    tables[props.tableId][props.position.name].amount = currentAmount + n;
  }
};
</script>

<template>
  <div className="{styles.position}">
    <label for="{position.name}">{{ position.name }}</label>
    <div className="{styles.optionsContainer}">
      <input
        className="{styles.input}"
        type="number"
        id="{position.name}"
        min="0"
        value="{orderPositions[position.name].amount}"
        @input="(e) => {
            // @ts-ignore
            const amount = e.target.value as number;
            // @ts-ignore
            tables[tableId][position.name].amount = amount;
          }"
      />
      <button
        @click="
          (e) => {
            e.preventDefault();
            changeAmount(1);
          }
        "
        className="{styles.addButton}"
      >
        +
      </button>
      <button
        @click="
          (e) => {
            e.preventDefault();
            changeAmount(-1);
          }
        "
        className="{styles.subtractButton}"
      >
        -
      </button>
      <button
        @click="
          (e) => {
            e.preventDefault();
            isRequestModalOpen = true;
          }
        "
      >
        Request
      </button>
      <RequestModal
        v-if="isRequestModalOpen"
        :tableId
        :positionName="position.name"
        :tables
        v-model:isRequestModalOpen="isRequestModalOpen"
      />
      <button
        @click="
          (e) => {
            e.preventDefault();
            areAllergensOpen = true;
          }
        "
      >
        Allergens
      </button>
      <AllergensModal
        v-if="areAllergensOpen"
        :positionName="position.name"
        v-model:areAllergensOpen="areAllergensOpen"
      />
    </div>
  </div>
</template>
