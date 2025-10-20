<script setup>
import RequestModal from "./Position/RequestModal.vue";
import AllergensModal from "./Position/AllergensModal.vue";
import { ref, inject } from "vue";

const props = defineProps({
  position: { name: String, price: Number, ingredients: Array },
  tableId: Number,
});

const isRequestModalOpen = ref(false);
const areAllergensOpen = ref(false);
const tables = inject("tables");

const changeAmount = (n) => {
  const table = tables[props.tableId];
  console.log(table[props.position.name].amount);
  const currentAmount = table[props.position.name].amount;
  if ((currentAmount > 0 && n < 0) || n >= 0) {
    tables[props.tableId][props.position.name].amount = currentAmount + n;
  }
};
</script>

<template>
  <div class="position">
    <label :for="position.name">{{ position.name }}</label>
    <div class="optionsContainer">
      <input
        class="input"
        type="number"
        :id="position.name"
        min="0"
        :value="tables[tableId][position.name].amount"
        @input="
          (e) => {
            const amount = e.target.value;
            tables[tableId][position.name].amount = amount;
          }
        "
      />
      <button
        @click="
          (e) => {
            e.preventDefault();
            changeAmount(1);
          }
        "
        class="addButton"
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
        class="subtractButton"
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

<style scoped>
.position {
  display: flex;
  justify-content: space-between;
  gap: 1em;
  font-size: 1.4rem;
  font-weight: 500;
  margin-bottom: 0.4em;
}

.position button {
  padding: 0.2em 0.7em;
}

.optionsContainer {
  display: flex;
  gap: 0.25em;
  font-size: 1.4rem;
}

.optionsContainer * {
  font-size: 1.3rem;
}

.input::-webkit-inner-spin-button,
.input::-webkit-outer-spin-button {
  /* For Chrome, Safari, Edge, Opera */
  -webkit-appearance: none;
  margin: 0;
}

/* For Firefox */
.input {
  -moz-appearance: textfield;
  width: 3ch;
  text-align: right;
  color: black;
}

.finalButtonsContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 2.5em;
}

.orderButton,
.cancelButton {
  width: min-content;
}

.orderButton {
  font-size: 1.8rem;
  padding: 0.25em 0.5em;
}

.cancelButton {
  background: none;
  border: none;
  text-decoration: underline;
  margin-top: 1.2em;
  font-size: 1.1rem;
}
</style>
