<script setup lang="ts">
import { defineEmits } from "vue";
const emit = defineEmits(["update:isRequestModalOpen"]);

defineProps<{
  positionName: string;
  tableId: number;
  tables: any;
}>();
</script>

<template>
  <div className="{styles.requestModal}">
    <label className="{styles.requestLabel}" for="positionName + '-request'">
      Special request
    </label>
    <textarea
      @input="
        (e) => {
          // @ts-ignore
          const text = e.target.value;
          tables[tableId][positionName].request = text;
        }
      "
      value="{orderPositions[positionName].request}"
      rows="{7}"
      cols="{23}"
      id="positionName + '-request'"
      className="{styles.requestTextarea}"
    ></textarea>
    <button
      @click="
        (e) => {
          e.preventDefault();
          emit('update:isRequestModalOpen', false);
        }
      "
      className="{styles.requestSaveButton}"
    >
      Close
    </button>
  </div>
</template>
