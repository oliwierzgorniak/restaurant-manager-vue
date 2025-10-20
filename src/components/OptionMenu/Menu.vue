<script setup>
import Position from "./Menu/Position.vue";
import { menu } from "../../data";
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

const bill = inject("bill");

const getNewBill = () => {
  const newBill = [...bill];
  kitchenData.forEach((position) => {
    for (let i = 0; i < position.amount; i++)
      newBill.push({
        name: position.name,
        selected: true,
      });
  });
  return newBill;
};

const tableState = inject("tablesState");
const isOptionMenuOpen = inject("isOptionMenuOpen");
const emit = defineEmits(["update:isOrderOpen"]);

const handleOrder = (e) => {
  e.preventDefault();
  const kitchenData = getKitchenData();
  console.log(kitchenData);

  bill.value = getNewBill();
  tables[props.tableId] = { ...initialOrderPositions };

  tableState.value = "Busy";
  emit("update:isOrderOpen", false);

  isOptionMenuOpen.value = false;
};
</script>

<template>
  <div className="{styles.background}">
    <form className="{styles.form}">
      <fieldset v-for="categoryName in Object.keys(menu)" className="{styles.fieldset}">
        <h2 className="{styles.subtitle}">{{ categoryName }}</h2>
        <Position v-for="position in menu[categoryName]" :position="position" :tableId="tableId" />
      </fieldset>
      <div className="{styles.finalButtonsContainer}">
        <button
          @click="
            {
              handleOrder;
            }
          "
          className="{styles.orderButton}"
        >
          Order
        </button>
        <button
          @click="
            () => {
              emit('update:isOrderOpen', false);
              tables[props.tableId] = { ...initialOrderPositions };
            }
          "
          className="{styles.cancelButton}"
        >
          Cancel
        </button>
      </div>
    </form>
  </div>
</template>
