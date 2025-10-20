<script setup>
import { initialOrderPositions } from "@/data";
import Position from "./Menu/Position.vue";
import { menu } from "@/data";
import { inject } from "vue";
const tables = inject("tables");
const props = defineProps({
  tableId: Number,
});

const getKitchenData = () => {
  const kitchenData = [];
  const table = tables[props.tableId];
  Object.keys(table).forEach((positionName) => {
    const position = table[positionName];
    if (position.amount > 0) kitchenData.push({ name: positionName, ...position });
  });

  return kitchenData;
};

const bills = inject("bills");

const addToBill = (kitchenData) => {
  console.log(props.tableId);
  kitchenData.forEach((position) => {
    for (let i = 0; i < position.amount; i++)
      bills[props.tableId].push({
        name: position.name,
        selected: true,
      });
  });
};

const tableState = inject("tableState");
const isOptionMenuOpen = inject("isOptionMenuOpen");
const emit = defineEmits(["update:isOrderOpen"]);

const handleOrder = (e) => {
  e.preventDefault();
  const kitchenData = getKitchenData();
  console.log(kitchenData);

  addToBill(kitchenData);
  tables[props.tableId] = JSON.parse(JSON.stringify(initialOrderPositions));

  tableState.value = "Busy";
  emit("update:isOrderOpen", false);

  isOptionMenuOpen.value = false;
};
</script>

<template>
  <div class="background">
    <form class="form">
      <fieldset v-for="categoryName in Object.keys(menu)" class="fieldset">
        <h2 class="subtitle">{{ categoryName }}</h2>
        <Position v-for="position in menu[categoryName]" :position="position" :tableId="tableId" />
      </fieldset>
      <div class="finalButtonsContainer">
        <button @click="handleOrder" class="orderButton">Order</button>
        <button
          @click="
            () => {
              emit('update:isOrderOpen', false);
              tables[props.tableId] = { ...initialOrderPositions };
            }
          "
          class="cancelButton"
        >
          Cancel
        </button>
      </div>
    </form>
  </div>
</template>

<style scoped>
.background {
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
}

.form {
  background: var(--background-color);
  padding: 1em 2.8em 3em 2.8em;
  width: max-content;
  max-height: 90vh;
  overflow-y: auto;
}

.subtitle {
  font-size: 2rem;
  font-weight: 600;
  margin-top: 0.4em;
  margin-bottom: 0.5em;
}

.fieldset {
  border: none;
}

.finalButtonsContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1em;
  margin-top: 0.7em;
}

.orderButton {
  font-size: 1.7rem;
  margin-top: 1.2em;
  padding: 0.3em 0.5em;
}

.cancelButton {
  border: none;
  background: none;
  text-decoration: underline;
  font-size: 1.2rem;
  color: var(--text-color);
}
</style>
