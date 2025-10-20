<script setup lang="ts">
import Menu from "../components/OptionMenu/Menu.vue";
import { ref } from "vue";

const props = defineProps<{
  tableId: number;
  tableState: string;
  isOptionMenuOpen: boolean;
}>();

const emit = defineEmits(["update:tableState", "update:isOptionMenuOpen"]);

const isBillMenuOpen = ref(false);
const isOrderOpen = ref(false);

const handleReserveClick = () => {
  const newState = props.tableState === "Reserved" ? "Free" : "Reserved";
  emit("update:tableState", newState);
  emit("update:isOptionMenuOpen", false);
};
</script>

<template>
  <div className="{styles.background}">
    <div className="{styles.optionMenu}">
      <button @click="isOrderOpen = true">Order</button>
      <button @click="isBillMenuOpen = true">Bill</button>
      <!-- <BillMenu v-if="isBillMenuOpen" :tableState="tableState" /> -->
      <button v-if="tableState != 'Busy'" @click="handleReserveClick">
        {{ tableState === "Reserved" ? "Unreserve" : "Reserve" }}
      </button>
      <button @click="emit('update:isOptionMenuOpen', false)" class="{styles.optionMenuClose}">
        cancel
      </button>
    </div>
    <Menu v-if="isOrderOpen" :tableId v-model:isOrderOpen="isOrderOpen" />
  </div>
</template>
