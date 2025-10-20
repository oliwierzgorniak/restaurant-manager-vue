<script setup>
import { defineEmits } from "vue";
const emit = defineEmits(["update:isRequestModalOpen"]);

defineProps(["positionName", "tableId", "tables"]);
</script>

<template>
  <div class="requestModal">
    <label class="requestLabel" :for="positionName + '-request'"> Special request </label>
    <textarea
      @input="
        (e) => {
          const text = e.target.value;
          tables[tableId][positionName].request = text;
        }
      "
      rows="{12}"
      cols="{40}"
      id="positionName + '-request'"
      class="requestTextarea"
      >{{ tables[tableId][positionName].request }}</textarea
    >
    <button
      @click="
        (e) => {
          e.preventDefault();
          emit('update:isRequestModalOpen', false);
        }
      "
      class="requestSaveButton"
    >
      Close
    </button>
  </div>
</template>

<style scoped>
.requestModal {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: var(--background-color);
  padding: 2em;
  border: 0.1em solid var(--text-color);
  text-align: center;
  display: flex;
  flex-direction: column;
}

.requestModal .requestLabel {
  font-size: 2rem;
  margin-bottom: 0.5em;
  font-weight: 550;
}

.requestTextarea {
  margin-top: 0.5em;
  font-size: 1.3rem;
  padding: 0.5em;
  color: black;
  width: 27rem;
  aspect-ratio: 3 / 1;
}

.requestSaveButton {
  font-size: 1.6rem;
  padding: 0.3em 0.7em;
  margin-top: 1em;
}
</style>
